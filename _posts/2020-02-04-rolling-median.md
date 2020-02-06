---
layout: post
title: Computing rolling median in Go
---

Back in the day when I was teaching myself how to ~~program~~ land a programming
job, I'd spend quite some time learning basic data structures such as queues,
stacks, heaps, linked lists, etc. To my disappointment though, now that I am
employed in tech I almost never get a chance to implement these no more.
Reasons are: I can often use a library or I simply don't need these ingredients
as they are too "low-level".

Recently though I have had a chance to brush up on my Computer Science 101
knowledge, and weeks go by but I am still excited. Thus I bring the
excitement to you, my dear reader!

The story begins in me having to make the following computation in go:

{% highlight python %}
ser.rolling(period=30).median() # this is python's Pandas library, N=30
{% endhighlight %}

i.e. computing [rolling/moving median][ma] of a series of timestamped numbers.
One of the reasons I like coding in Go sometimes is because it forces you to
implement things like that from scratch, rather than rely on someone else's
code. It's problems like these that got me enthusiastic about entering this
profession (as opposed to having to peruse [Django][django] documentation: a read that
doesn't have a great ending).

----

Without further ado, let's begin our rolling median implementation. Start with
an underlying data structure that holds our timestamped numbers:

{% highlight go %}
// Data holds Value & Time when value was observed
type Data struct {
	Value float64
	Time  time.Time
}
{% endhighlight %}

When computing rolling median, we will need to remember last N (where N is the
size of the window) entries in a sorted order. The fact that elements are
sorted allows us to quickly find the median by picking the element in the
middle. Go provides a handy `sort` library that allows one to use [binary search][binsearch]
to insert & delete elements from a slice that's already sorted:

{% highlight go %}
// SortedFloatSlice assumes elements are sorted
type SortedFloatSlice []float64

// Insert into slice maintaing the sort order
func (f SortedFloatSlice) Insert(value float64) SortedFloatSlice {
	i := sort.SearchFloat64s(f, value)
	n := append(f, 0)
	copy(n[i+1:], n[i:])
	n[i] = value
	return n
}

// Delete from slice maintaing the sort order
func (f SortedFloatSlice) Delete(value float64) SortedFloatSlice {
	i := sort.SearchFloat64s(f, value)
	n := append(f[:i], f[i+1:]...)
	return n
}

// Median of the slice
func (f SortedFloatSlice) Median() float64 {
	if len(f)%2 == 1 {
		return f[len(f)/2]
	}
	return (f[len(f)/2] + f[len(f)/2-1]) / 2
}
{% endhighlight %}

We have all the components to make the computation possible. Just like pandas
we pre-fill the result with `NaN`'s where computation is impossible. We
initialise the sliding window at the start of the series, and keep advancing
the window one element at a time, whilst keeping the window sorted

{% highlight go %}
func movingMedian(series internal.Metrics, period int) internal.Metrics {
	res := make(internal.Metrics, len(series))

	window := make(SortedFloatSlice, 0, period-1)
	for _, v := range series[:(period - 1)] {
		window = append(window, v.Value)
	}
	sort.Float64s(window)

	median := float64(0)
	for i, v := range series {
		if i < period-1 {
			median = math.NaN()
		} else {
			if i-period >= 0 {
				window = window.Delete(series[i-period].Value)
			}
			window = window.Insert(v.Value)
			median = window.Median()
		}

		res[i] = internal.Metric{
			Timestamp: v.Timestamp,
			Value:     median,
		}
	}

	return res
}
{% endhighlight %}


---

Complexity analysis: we have to do insert/delete from a sorted array at each
step, which is O(window). Since we have N steps the resulting complexity is
O(N * window). Can we do better? Turns out the canonical algorithm for computing
a rolling median is to keep window elements in a [skip list][skiplist] instead.
Skip list allows one to insert, delete and random access sorted elements in
logN times, thus the complexity of the canonical algorithm is lower:
O(N * log(window)). However for windows of small sizes, I feel like the advantage
of skip lists is not worth the cost of much more complex implementation.

This concludes the code & the blog post. Hurray!

TL;DR I used a sorted array to make the computation.

TL;DR;TL;DR I am (still) a massive nerd.


[ma]: https://en.wikipedia.org/wiki/Moving_average
[queue]: https://en.wikipedia.org/wiki/Queue_(abstract_data_type)
[django]: https://www.djangoproject.com/
[binsearch]: https://en.wikipedia.org/wiki/Binary_search_algorithm
[skiplist]: https://en.wikipedia.org/wiki/Skip_list
