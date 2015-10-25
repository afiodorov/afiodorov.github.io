---
layout: post
title: Elementary hypothesis testing or proving Raf wrong
---

Context (paraphrasing):

~~~
Raf: No couples in the Chalkdust team!
Me: Why?
Raf: Had one bad experience before.
Me: So your sample size is one?
Raf: No, I have a million of examples when a girl and a boy work well together
when they are not a couple, so my sample size is 10^6!
Me: No, your sample size is still one. I'm going to figure it out and write a blog post about it!
~~~

------

Really? A million of negative examples now count? Oh well, let's go through
this anyway like mathematicians. Notation:

\\(A = 0\\) stand for "a male and a female are not romantically involved",

\\(A = 1\\) stand for "a male and a female are romantically involved".

\\(B = 0\\) stand for "a male and a female work together well"[^1],

\\(B = 1\\) stand for "a male and a female don't work together well",


So the data, \\(D\\), is just a collection of tuples of the form \\((A, B)\\)
such that we have \\(10^6\\) examples of \\((0, 0)\\) and just \\(1\\) \\((1,
1)\\).

Finally, \\(H_1 := B = A \\), i.e. a male and a female work well iff they are
not a couple.

Now good statistics dictates specifying an alternative hypothesis, \\(H_2\\),
we are testing \\(H_1\\) against.

What could \\(H_2\\) be? Just a negation: \\(B \neq A\\) is not good enough,
as it doesn't tell us what to expect. In other words, it doesn't specify
\\(P(D | H_2)\\).

Why is this the case? Just because we are supposing that a couple could work
well together, we don't say at all how often this happens. There is nothing
in \\(H_2\\) that allows us to make such judgement. Instead, let us modify
\\(H_2\\) to be a bit more specific:

$$P(B = 0 | A = 0, H_2^{'}) = 1 \,,$$

so if a male and a female are not romantically involved they *will* work
together well. And

$$P(B = 1 | A = 1, H_2^{'}) = q \,,$$

for some \\(0 < q < 1\\) to be picked later. Since our \\(H_2^{\'}\\) is now
much more specific, we can meaningfully talk about \\(P(D | H_2^{\'})\\), which
is just

$$P(D | H_2^{'}) = 1^{10^6} q^{1} = q,$$

simply by the definition of \\(H_2^{\'}\\). We are now at the stage when
we can deploy a simple hypothesis testing procedure to figure out what it is
going on. Via the elementary use of Bayes' rule we can show that

$$ \log \left( \frac{P(H_1 | D)}{P(H_2^{'} | D)} \right) =
\log \left( \frac{P(H_1)}{P(H_2^{'})} \right) +
\log \left( \frac{P(D | H_1)}{P(D | H_2^{'})} \right) \,,
$$

where

$$
\frac{P(H_1)}{P(H_2^{'})},
$$

is simply how much you believed in \\(H_1\\) vs \\(H_2\\) prior to seeing
the data and


$$ \frac{P(H_1 | D)}{P(H_2^{'} | D)}, $$

is how much you ought to believe after observing \\(D\\). Hence the quantity
of interest is

$$
\log \left( \frac{P(D | H_1)}{P(D | H_2^{'})} \right) \,,
$$

known as Bayes factor which tells us in what direction we should move our
believes about the validity of \\(H_1\\) and \\(H_2^{\'}\\) once we see the
data.

Now Rafael's hypothesis, \\(H_1\\), supports the data perfectly well, i.e.
\\(P(D | H_1) = 1\\), hence


$$
\log \left( \frac{P(D | H_1)}{P(D | H_2^{'})} \right)
= \log \left( \frac{1}{q} \right) \,,
$$

First thing to note is that the above answer is *independent of the number of
negative examples we have seen*. Even if our data, \\(D\\), contained
\\(10^{10^{10}}\\) tuples \\((0, 0)\\) it would have changed nothing. So this
hypothesis testing agrees with our common sense: the number of negative
examples doesn't teach us anything about the inference about positive examples.

Observing a trillion of trillion of times how people don't eat substance
\\(X\\) and don't get cancer tells us nothing about getting cancer if \\(X\\)
is to be consumed.

So, no, Rafael, your *effective* sample size is still \\(1\\) and not \\(10^6 +
1\\). By effective I mean the size of the sample that is actually relevant
to the inference of interest.

---

Finishing the example, it is more intuitive to work with rescaled logs based
\\(10\\), so

$$10 \log_{10} \left( \frac{1}{q} \right) = - 10 \log_{10} q \,,$$

now what should \\(q\\) be? Well... I am too lazy to read the research on the
matter of how often couples work well together. So if I assume that half of
the time, i.e. \\(q= 1 /2\\), then observing just one positive example
yields \\(10 \log_{10} 2 \approx 3\\) dHart units in favour of \\(H_1\\).
However \\(3\\) dHarts is "barely worth mentioning" according to [wiki][wiki].
So we arrived at a more formal way of saying why sample size \\(1\\) isn't
significant. See also:

* [Generalizing From One Example][lw]

The end.

[wiki]: https://en.wikipedia.org/wiki/Bayes_factor
[lw]: http://lesswrong.com/lw/dr/generalizing_from_one_example/
[^1]: not only they are capable of fulfilling the tasks needed, they also don't
      make the surrounding team any less productive
