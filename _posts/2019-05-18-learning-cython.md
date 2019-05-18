---
layout: post
title: Parallelising Machine Learning in Python
---

About 3 years back when I got my first job in Machine Learning I would run most
of my analysis on a 32 cores machine. 32 cores are perfect for running things
in parallel... so I thought. But then I learned about [GIL]. Basically, Python,
despite being one of the most popular languages for data science, is ["broken"]
when it comes to multithreading: cpu-bound threads can only use one core at a
time.

This leads to certain [embarrassingly parallelisable][par] code to execute
sequentially. In this blog post I want to compute a sum of each column in a
large numpy array. An obvious way to parallelise this is to compute the sum for
each column in parallel and then aggregate the result. How can this be achieved?

------

## "Hack 1": [multiprocessing]

Instead of using threads we [fork] the main process. The main disadvantage of
this method is that each process should receive a copy of you data. So if we
are operating on a 20gb dataframe, a naive execution of 32 processes will end
up consuming 640gb of memory. This, of course, will result in `MemoryError` in
cases when it's worth parallelising in the first place. However, it turns out
that Linux uses [copy-on-write] mechanism when creating new processes. This
means that we can share memory between processors as long as we don't modify
the underlying dataframe. In practice I found that storing a numpy array in a
global variable (a list or a dict) and then starting processes allows me to
utilise all cores and still write code in pure Python, see example below:


{% highlight python %}
from multiprocessing import Pool
import numpy as np

data = {}

def sum_col(col_num):
    arr = data['arr']
    return np.sum(arr[:, col_num])

def sum_multiprocessing(arr):
    data['arr'] = arr

    with Pool() as p:
        l = p.map(sum_col, range(arr.shape[1]))

    del data['arr']
    return np.array(l)
{% endhighlight %}

This works, but the overhead of setting up all the processes often kills the
performance gain. Moreover, in my experience I found the copy-on-write
mechanism not very reliable: certain aggregate operations that should be
read-only would sometimes trigger Linux to make copies. Nevertheless this
methods has served me well and it is a good starting point.

Let's see some timings. First let's create a random numpy array and convert it
to Fortran-contiguous, i.e. make it so that columns are stored next to one
another:

{% highlight python %}
A = np.random.rand(30000, 10000)
B = np.asfortranarray(A)
{% endhighlight %}

See this [answer] on StackOverflow if you are not sure why the conversion. Finally, let's time the execution:

{% highlight python %}
> %timeit B.sum(axis=0)
146 ms ± 14 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

> %timeit sum_multiprocessing(B)
649 ms ± 75.5 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)

{% endhighlight %}

So our execution is slower than a simple sum performed by numpy (which is
single-threaded, according to `htop` anyway) and that is despite the fact that
`sum_multiprocessing` utilises all 4 of my cores. One advantage of the code
above is that it could work just as well with Pandas' dataframes just fine, as
everything is done in pure python.

## Another approach: [Cython]

Let's see if we can actually beat numpy's `np.sum()`. Since the above is a bit
of a hack and only works on Linux, how do libraries like [scikit-learn] utilise
all cores? They use Cython which allows one to write multithreaded code without
worrying about the GIL. Cython also has a [good way] to interact with numpy
arrays. Without further ado, lets jump into the code and the result:

{% highlight python %}
%%cython
# distutils: extra_compile_args=-fopenmp
# distutils: extra_link_args=-fopenmp

import numpy as np
from cython.parallel import prange
cimport cython

@cython.boundscheck(False)
@cython.wraparound(False)
cdef double sum_col(double[::1, :] X, Py_ssize_t col_num) nogil:
    cdef double sum = 0
    cdef Py_ssize_t i

    for i in range(X.shape[0]):
        sum += X[i, col_num]
    return sum

@cython.boundscheck(False)
@cython.wraparound(False)
def sum_cython(double[::1, :] X):
    res = np.zeros(X.shape[1])
    cdef double[:] res_view = res
    cdef Py_ssize_t i
    for i in prange(X.shape[1], nogil=True):
        res_view[i] = sum_col(X, i)

    return res

> %timeit B.sum(axis=0)
143 ms ± 17.9 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

> %timeit sum_cython(B)
113 ms ± 12.1 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)

{% endhighlight %}

OK, looks like my Cython multithreaded implementation is somewhat faster. First
of all, why is `np.sum` so difficult to beat? I strongly suspect it's using
[vector instructions].

Now some comments on the code. My original version of `cdef double sum_col` was just

{% highlight python %}
return np.sum(X[:, col_num])
{% endhighlight %}

But this won't compile. The error is: Accessing Python attribute not allowed
without gil. Looks like the result of `np.sum()` is a numpy array, which is a
python object. But we can't construct a python object without the GIL as
cython can't talk to the python interpreter. This means that Cython is quite
limited: I have to reimplement even the basics in order to benefit from
multithreading. For exact same reason I can't just have

{% highlight python %}
# original: cdef double[:] res_view = res
# original: res_view[i] = sum_col(X, i)
res[i] = sum_col(X, i)
{% endhighlight %}

because `res` is a python object. Luckily Cython allows one to access numpy
arrays by having [memoryviews], see `res_view` above. Further comments on the
code:

{% highlight python %}
@cython.boundscheck(False)
@cython.wraparound(False)
{% endhighlight %}

lines just speed up the execution by making the code less safe, I only added
those once I had a working implementation. Note as well that the code can't
easily be generalised to Pandas' dataframes as they are python objects.

# Conclusion

Whilst Cython provides a way for machine learning practitioners to utilise all
cores, it requires writing a lot of code from scratch and converting
dataframes to numpy arrays. Perhaps such complexity is only warranted once you
already know very well what you want to compute. If you are still
investigating, `multiprocessing` sounds like a better approach.

So, natural question occurs, why use Python at all? Why don't I switch to a
language that is much more adapted for multithreading? The truth is I don't
have to parallelise things that often: I can often get away with subsampling
data instead and running my experiments only on a small part of the data. In
the end, the answer must be that I prefer leveraging a well-established machine
learning infrastructure that Python has over using something more obscure, even
if it is better.


[GIL]: https://wiki.python.org/moin/GlobalInterpreterLock
["broken"]: https://www.youtube.com/watch?v=ph374fJqFPE
[par]: https://en.wikipedia.org/wiki/Embarrassingly_parallel
[fork]: https://en.wikipedia.org/wiki/Fork_(system_call)
[multiprocessing]: https://docs.python.org/3.4/library/multiprocessing.html
[copy-on-write]: https://en.wikipedia.org/wiki/Copy-on-write
[answer]: https://stackoverflow.com/questions/26998223/what-is-the-difference-between-contiguous-and-non-contiguous-arrays
[Cython]: https://cython.org/
[good way]: https://cython.readthedocs.io/en/latest/src/tutorial/numpy.html
[vector instructions]: https://en.wikipedia.org/wiki/Vector_processor
[memoryviews]: https://cython.readthedocs.io/en/latest/src/userguide/memoryviews.html
[scikit-learn]: http://scikit-learn.org
