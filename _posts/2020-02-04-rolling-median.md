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

Moreover, when dealing with computation of the rolling median, we ought to remember
the insertion order of the numbers in the rolling window. In other words, we
need a fixed size [queue][queue]. This necessary for advancing the rolling
window one element at a time, i.e. deleting the oldest element & inserting a
new one:

{% highlight go %}
// FloatQueue is FIFO implementation
type FloatQueue []float64

// Push to queue
func (fs FloatQueue) Push(item float64) FloatQueue {
	return append(fs, item)
}

// Pop from queue
func (fs FloatQueue) Pop() (item float64, queue FloatQueue) {
	return fs[0], fs[1:]
}
{% endhighlight %}

Looks like we have the components to make the rolling window possible. We
insert into the window remembering the insertion order using the `FloatQueue`
and keeping the last N elements sorted using the `SortedFloatSlice`:

{% highlight go %}
// FixedSizeSortedQueue remembers the insert order & keeps last N elements sorted
type FixedSizeSortedQueue struct {
	Queue  FloatQueue
	Sorted SortedFloatSlice
}

// Push will insert a new element, remove oldest & keep the slice sorted
func (s *FixedSizeSortedQueue) Push(item float64) {
	removed, newQueue := s.Queue.Pop()
	newQueue = newQueue.Push(item)
	newSorted := s.Sorted.Delete(removed)
	newSorted = newSorted.Insert(item)
	s.Queue = newQueue
	s.Sorted = newSorted
}

// NewFixedSizeSortedQueue initialises the queue of fixed order
func NewFixedSizeSortedQueue(values []float64) FixedSizeSortedQueue {
	sorted := make([]float64, len(values))
	copy(sorted, values)
	sort.Float64s(sorted)
	return FixedSizeSortedQueue{
		Queue:  values,
		Sorted: sorted,
	}
}
{% endhighlight %}

Finally we have all the components to make the computation possible. Just like
pandas we pre-fill the result with `NaN`'s where computation is impossible. We
initialise the sliding window at the start of the series, and keep advancing
the window one element at a time, whilst keeping the window sorted & memorising
which element should be deleted at the next step:

{% highlight go %}
func movingMedian(series []Data, period int) []Data {
	res := make([]Data, len(series))

	initialWindow := make([]float64, 1, period)
	initialWindow[0] = 0 // we need one extra element that will be ignored
	for _, v := range series[:(period - 1)] {
		initialWindow = append(initialWindow, v.Value)
	}
	queue := NewFixedSizeSortedQueue(initialWindow)

	median := float64(0)
	for i, v := range series {
		if i < period-1 {
			median = math.NaN()
		} else {
			queue.Push(v.Value)
			median = queue.Sorted.Median()
		}

		res[i] = Data{
			Value: median,
			Time:  v.Time,
		}
	}

	return res
}
{% endhighlight %}


---

This concludes the code & the blog post. Hurray! (So much code though that does
what python does in one line)

TL;DR I used a queue & a sorted array to make the computation.

TL;DR;TL;DR I am (still) a massive nerd.


[ma]: https://en.wikipedia.org/wiki/Moving_average
[queue]: https://en.wikipedia.org/wiki/Queue_(abstract_data_type)
[django]: https://www.djangoproject.com/
[binsearch]: https://en.wikipedia.org/wiki/Binary_search_algorithm
