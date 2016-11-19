---
layout: post
title: How much sampling is enough? Bayesian case-study.
---

I was asked the following question yesterday:

> Imagine a factory produces defective products. We grab \\(n\\) products from
> the production line and find that \\(n_d\\) of them are defective, thus we
> extrapolate that \\(n_d / n \\) percentage of products is defective.
> However, how confident are we about our estimate? How many products should we
> sample to reach a satisfactory level of confidence?

Aha! The universe has prepared me for these kind of questions! Be prepared to
be converted to Bayesianism because this example is particularly well-suited
for such framework.

Let's begin: assume that there are \\(N\\) products in total and whether a
product is defective is modelled by Bernoulli distribution with mean \\(p\\).
Thus, after we examine \\(n\\) products, the probability distribution describing the rest of
products is Binomial with parameters \\( p \\) and \\( N -n \\), also written as \\( Bin(N - n, p))\\).

However we don't know what \\( p \\) is! Bayesians say: well, let's be honest
about this fact and put another probability distribution on \\(p\\) to reflect
our uncertainty.

What probability distribution should we use to describe such uncertainty? Well, in theory,
anything we deem appropriate. In practice, people stick to so-called [conjugate
prior distributions][conj] because they are *very* mathematically convenient and often
are flexible enough.

In this particular situation, the conjugate prior distribution is [Beta distribution][beta].
This means that our uncertainty over \\(p\\) could be represented in a variety of ways, see examples below:
![beta distributions](https://upload.wikimedia.org/wikipedia/commons/f/f3/Beta_distribution_pdf.svg)

Beta distribution has 2 parameters: \\(\alpha\\) and \\(\beta\\). By fixing
these two parameters we have a particular instance of such distribution. Above
each curves represent a different instance. For example if you pick a purple
curve you believe that \\(p\\) is around \\(0.5\\), but you don't rule out that \\(p\\) could also be
\\(\le 0.1\\) or \\(\ge 0.9\\), both with equal and small probability.

So, prior to seeing any defective examples, what is a reasonable representation
of your uncertainty over \\( p\\)? Such representation is often known as
**prior** distribution, and the exact choice of prior is subject to some
debate. However, skipping the unnecessary complications, let's just say that
uniform prior over \\( p \\) is reasonable. Uniform distribution happens to be
a particular instance of Beta distribution, with parameters \\(\alpha=1\\) and
\\(\beta=1\\), also written as \\(Beta(1, 1)\\).

And once we observe \\(n_d\\) defective products and \\(n_g\\) non-defective
products, how should we update our belief about \\(p\\)? Easy! Turns out, using
*maths*, one can discover that the new distribution is just
\\(Beta(n_d + 1, n_g + 1)\\). This distribution now incorporates our
observation and thus is referred to as a *posterior*. The fact that both prior
and posterior distribution happen to be instances of Beta distribution is the
reason such distribution is often picked as a prior.

Finally, let us now derive the variance of \\(Bin(N-n, p)\\). This variance
represents our uncertainty about examining whether the rest of the \\(N-n\\)
products are defective. Remember that when we talk about the variance of
\\(Bin(N-p, p)\\) there are 2 sources of uncertainty:

  1. The variance of Binomial distribution for any fixed \\(p \\).
  2. The variance that comes from the fact that \\(p \\) is itself a random variable.

Note that 2. would be missing from frequentist treatment of the problem, you'd say something like

\\(Var[Bin(N-n, \hat{p})] = (N-n)\hat{p}(1 - \hat{p}) \\), where

\\( \hat{p} = n_d / n \\), and be done with the problem. However in such
treatment it is not possible to estimate the effect of the sample size,
\\(n\\), on your final uncertainty - which is deeply unsatisfactory.

However, guided by the formula of [conditional variance][cond], under Bayesian
treatment, we can say that

$$
\begin{align*}
Var[Bin(N - n, p)] &= \mathbb{E}[Var[Bin(N-n, p)]] + Var[\mathbb{E}[Bin(N-n, p)]]
\\
&= \mathbb{E}[(N-n)p(1-p)] + Var[(N-n)p]
\\
&= (N-n) \mathbb{E}[p(1-p)] + (N-n)^2 Var[p].
\end{align*}
$$

where outmost expectations and variance are taken with respect to *posterior*,
i.e. \\(Beta(n_d + 1, n_g + 1)\\).

Note that \\(Var[p]\\) can easily be looked up and it is

$$
\frac{(n_d + 1)(n_g + 1)}{(n_d + n_g + 2)^2(n_d + n_g + 3)} = \frac{(n_d + 1)(n_g + 1)}{(n + 2)^2(n+3)}.
$$

So what is left to compute is \\(\mathbb{E}[p(1-p)]\\):

$$
\begin{align*}
\mathbb{E}[p(1-p)] &= \int \frac{p^{(n_d + 1)} (1-p)^{(n_g + 1)}}{B(n_d + 1, n_g + 1)}
\\
&= \frac{B(n_d + 2, n_g + 2)}{B(n_d + 1, n_g + 1)}
\\
&= \frac{(n_d + 1)(n_g +1)}{(n_d + n_g + 2)(n_d + n_g + 3)}
\\
&=
\frac{(n_d + 1)(n_g + 1)}{(n + 2)(n+3)} .
\end{align*}
$$

Thus we get that

$$
Var[Bin(N -n, p)] = \frac{(n_d + 1)(n_g + 1)}{(n+2)(n+3)} \left[ N-n + \frac{(N-n)^2}{n+2} \right].
$$

Let's put some numbers in!

If \\(N = 10^6 \\), \\(n = 1000 \\) and \\(n_d = n_g = 500 \\), then \\( std(Bin(10^6 - 10^3, p)) \approx 1.58 * 10^4 \\).

So we anticipate to get \\(O(10^4)\\) products wrong if we extrapolate to
\\(10^6\\) examples after measuring \\(1000\\)... which sounds about right to
me.

And when should you stop sampling? Well, remember, that even if you knew
\\(p\\) precisely, the standard deviation would still be of order \\( 1000
\\), due to the variance of the Binomial distribution. So once you get close
to this order, sampling more won't be of much use. So we want the largest term in the above
expression, \\(\sqrt{(N-n)^2  / (n+2)}\\), to be of order \\(10^3\\), where the
square root comes from switching from variance to standard deviation, thus I am
getting that sampling beyond \\(10^5\\), i.e. 10% of the data, is unnecessary.


[conj]: https://en.wikipedia.org/wiki/Conjugate_prior
[beta]: https://en.wikipedia.org/wiki/Beta_distribution
[cond]: https://en.wikipedia.org/wiki/Conditional_variance
