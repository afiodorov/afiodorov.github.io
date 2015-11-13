---
layout: post
title: Causality, wisdom of a crowd, scary but obvious thoughts
category: probability-the-logic-of-science
---

Prerequisite: [there is no such thing as a biased coin]({% post_url 2015-11-12-unbiased-coin %}).

*Theorem (Central limit theorem)*. Let \\(X_1, \dots, X_n\\) be independent and
identically distributed random variables with mean \\(0\\), then

$$\frac{X_1 + \cdots + X_n}{n} \xrightarrow{d} N(0, std(X)^2).$$

Where \\(std(X)\\) is just a standard deviation of \\(X\\).

I hope you vaguely remember how it works: take any experiment, repeat it many
times independently and Gaussian distribution comes out. **Magic**.

Now let's try to apply it. The following is known as [Wisdom of the
crowd][wisdom], although it is also known as Emperor's height fallacy:

> Supposing that each person in China surely knows the height of the Emperor,
> \\(h\\), to an accuracy of at least \\(\pm 1\\) meter; if there are \\(N = 1
> 000 000 000\\) inhabitants, then it seems that we could determine his height
> to an accuracy at least as good as \\(1/\sqrt{1 000 000 000}\\) m = \\(3 ×
> 10−5\\) m = \\(0.03\\) mm, merely by asking each person’s opinion and
> averaging the results.
>
> -- <cite>E.T. Jaynes</cite>

So what is going on with our misapplication of CLT?

Let us first figure out what independent assumption means.  The formal
definition is the following:

$$P(X_1 | X_2) = P(X_1).$$

So, knowing the value of the first experiment doesn't change the probability
of the second experiment.

However, this is where the way you perceive probability starts to *matter*. I
can't wait to highlight how all the derision of Bayesianism as something
metaphysical, unimportant and just philosophical is misguided.

## I. Orthodox statistics

If your brain was twisted by modern teachings of probability theory you start
thinking that there's something random in this universe which drives the
outcomes of the experiments.

Take flip of a coin. If you say that coin has an intrinsic probability \\(p\\)
of coming up heads, you say that the flips of a coin are driven by some random
process and each experiment is a realisation of such randomness, where random
process is something that can not predicted by anyone in this world.

If you think I am straw manning, I am not. And it's no surprise this idea was
not favoured by physicists.

Question: are guesses of two people (who didn't not communicate to each other)
independent?

If first guess, \\(X_1\\), was simply a realisation of some random process,
be it positions of the atoms in the universe or anything else, it *can not*
predispose another person to guess anything related to \\(X_1\\). If it could,
then what you are effectively saying is that there's a physical causal effect
between individual guesses. So mutterings of the first responded changes the
minds of the respondents afterwards. Nonsense. (*No* communication between the
respondents is assumed.)

Therefore, in orthodox statistics, individual guesses are independent.

This conclusion is absurd: if the respondents keep overestimating the Emperor's
height, you should start taking this data into account: as it is likely that
the next guess will also be over. But, not in orthodox stats, where
independence is the same as "no physical cause".

So what stopping us from applying Central limit theory (CLT) is the identically
distributed assumption.

So assign a probability distribution, say \\(\mathcal{R}(\mu, \sigma^2)\\) such
that the sequences of guesses \\(X_1, X_2, X_3, \dots, X_n\\) came from it. But
now the CLT applies and we can say that

$$\frac{X_1 + \dots + X_n}{n} \approx N(\mu, \sigma^2 / n)$$

So as long as \\(\mu\\) is close to the real height of the Emperor, \\(h\\), we
arrive at a supposedly incredibly accurate estimation.

Since  \\(\mu\\) is unknown, it is estimated by \\(\hat{\mu}\\):

$$\hat{\mu} := \frac{X_1 + \dots + X_n}{n}.$$

And, orthodoxy concludes, that Wisdom of a Crowd is correct as long as

* no communication
* \\(\hat{\mu} = h\\)

Last condition can also be rephrased

Last condition is "you don't say". Because it lead us to a tautology: wisdom
is correct as long as it's correct. No shit, Sherlock.

[wisdom]: https://en.wikipedia.org/wiki/Wisdom_of_the_crowd
