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

**Edit.**

Not quite the end yet.

*Objection 1*:

~~~
Raf: Only that by picking the H2' and the test used as desired, is not really an argument, is it?
~~~

----

Perhaps I didn't motivate \\(H_2^{\'}\\) enough. Let's be a little bit more
careful. By the Cox's theorem we know that
there exist a tuple \\((p_1, p_2) \in [0, 1]^2\\) such that

\begin{align}
P(B = 0 | A = 0, H) = p_1 \,,
\\
P(B = 1 | A = 1, H) = p_2 \,,
\end{align}

for any \\(H\\). In particular, \\(H_1\\) is just \\(H(1, 1)\\). Now it is up
to *us* to pick what \\(H(p_1, p_2)\\) we want to test \\(H(1, 1)\\) against.

Skipping the unnecessary details, it is easy to be convinced that by performing
an elementary hypothesis testing to compare \\(H_1\\) vs \\(H(p_1, p_2)\\) when
\\(p_1 < 0.99\\) and \\(p_2\\) arbitrary we can quickly arrive at a very high
Bayes Factor in favour of \\(H_1\\) due to the huge number of negative
examples. The intuition is simple: after seeing \\(10^6\\) negative examples we
must be much more convinced in believing that \\( p_1 \approx 1\\) then we were
before.

So unless you have some very strong prior against \\( p_1 \approx 1\\), there is
no reason to stick with \\( p_1 \ll 1\\) after seeing your knowledge contradicted
again and again.

So I could just leave \\(p_1\\) to be very high compared to \\(1\\), but that
would only complicate things with no benefit whatsoever, as \\(p_1\\) has to be
picked high enough so that its contribution to the Bayes factor is negligible
anyway.

----

Above is also *irrelevant* as we only care about the inference based on
positive examples in the first place. All we are interesting in, for the
purpose of the discussion, is whether a couple, picked from the population,
works together well. Hence we are only interested in changing the \\(p_2\\)
part of \\(H_1\\) in the first place.

---

Back to \\(H_2^{\'} = H(1, q)\\):

$$P(B = 1 | A = 1, H_2^{'}) = q \,,$$

This is just my prediction of a couple working well, which exists by the
Cox's theorem. Remember, plausibility of a statement is between \\(0\\) and
\\(1\\)? Now I left it undefined. It could be \\(1/2\\) for me, or
\\(1/3\\) for you. This is parametrised for the following reason:
it turns out, that despite the large number of negative examples, only the
*positive examples* are capable of adjusting your \\(q\\). Hence addressing the
dispute over the effective sample size without actually resorting to specifying
\\(q\\).

Note that the procedure of hypothesis testing via the means of the Bayes factor
is just a consequence of Bayes' theorem, which, in turn, is derived from Cox's
theorem, which is derived from the axioms I showed to you. Hence my test isn't
"desired", it is simply consistent with the axioms behind the Cox's theorem,
therefore it is *necessary* and not simply desired.

Finally Bayes' factor method is very convenient as it is capable of *adjusting
your prior* but doesn't actually care about your *current* prior. Hence we
don't actually need to argue about \\(q\\) here at all, it's the fact that the
update considers the only example that you have is relevant here: since it
only cares about \\(1\\) example, the effective sample size is just \\(1\\).

[wiki]: https://en.wikipedia.org/wiki/Bayes_factor
[lw]: http://lesswrong.com/lw/dr/generalizing_from_one_example/
[^1]: not only they are capable of fulfilling the tasks needed, they also don't
      make the surrounding team any less productive
