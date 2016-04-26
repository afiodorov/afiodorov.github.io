---
layout: post
title: Computing feature contributions for any binary classifier in an unbalanced case
---

State of progress: *eternal draft*. Has this already been published by someone
else: *unknown*.

## Why model interpretation?

A situation where an explanation is required behind a particular prediction
is very common. Example situations arise often in fraud detection and medical
setting.

## General method for arriving at feature contributions

The re-emergence of black box methods calls for new interpretability techniques
to be developed as interpretability is the main obstacle behind wider adoption
of such techniques.

## Notation

We are concerned with classifying \\( Y \in \\\{ 0, 1\\\}\\) given features
\\(X_1, X_2, \dots, X_p\\), where \\(p \\) stand for a number of features.

We have a trained classifier \\(f : \mathbb{R}^p \to [0, 1] \\) which
predicts \\(Y\\) by outputting probabilities. The nature of \\( f\\) isn't
important to us.

## Suggested approach: theory

Chapter 2 of the Elements of Statistical Learning book highlighted that
every classifier is trying to learn

$$
P(Y = y | X_1 = x_1, X_2 = x_2, \dots, X_p = x_p)
$$

hence we can think of \\( f(x_1, x_2, \dots, x_p) \approx P(Y | X_1 = x_1,
X_2=x_2, \dots, X_p = x_p) \\), where \\(x_1, x_2, \dots, x_p\\) are values
of corresponding features for a sample we are trying to predict.

Under the Naive-Bayes assumption of conditional independence applied to
features, we have

$$
P(y | x_1, x_2, \dots, x_p) =
\frac{1}{P(x_1, x_2, \dots, x_p)} P(y)
\prod_{i=1}^{p} P(x_i | y) \,,
$$

where events of type \\(X_i = x_i\\) have been abbreviated to \\(x_i\\) for
notational convenience.

Hence

$$
\frac{P(y | x_1, \dots x_{k-1}, x_{k+1}, \dots x_p)}{
P(y | x_1, x_2, \dots, x_p) } =
\frac{P(x_1, \dots x_{k-1}, x_{k+1}, \dots x_p)}{
P(x_1, x_2, \dots, x_p) } \frac{1}{P(x_k | y)}
$$

Substituting it back to the equation above, we get that

$$
P(y | x_1, x_2, \dots, x_p) = C(X) P(y) \prod_{k=1}^{p} p_{-k},
$$

where \\( C(X) \\) is a constant which depends on the sample's data only and

$$p_{-k} := \frac{P(y | x_1, x_2, \dots, x_p) }
{P(y | x_1, \dots x_{k-1}, x_{k+1}, \dots x_p)}$$

for any \\(1 \leq k \leq p\\).

Each \\(p_{-k}\\) can be interpreted as the relative change in the outcome
probability had we not observed the value of feature \\(X_k\\).

Now we can decompose our feature contributions, by taking a log:

$$
\log P(y | x_1, x_2, \dots, x_p) = bias + \sum_{i = 1}^{p} \log p_{-i}
$$

where bias term ensures equality and is equal to \\(\log (C(X)P(y)) \\).

## Feature importances: practice

Computing \\( P(y | x_1, x_2, \dots, x_p) \\) is straightforward, as it is
simply \\( f(x_1, x_2, \dots, x_p) \\). \\(p_{-k} \\)'s, however, provide a
bigger challenge.

For unbalanced classification (with negative examples being far more common),
we suggest using a typical value, \\( \bar x_k \\), of \\(X_k\\) amongst
negative examples to approximate \\( P(y | x_1, x_2, \dots x_{k-1}, x_{k+1},
x_p)\\) as \\( f(x_1, x_2, \dots, x_{k-1}, \bar x_k, x_{k+1}, x_p )\\). The
idea being that by feeding the most "typical value" of \\(X_k\\) to our
classifier, it treats \\(\bar x_k \\) as uninformative for predicting positive
class, effectively ignoring \\( \bar x_k \\) all together.

Thus the method follows:

1. Compute \\( \bar x_i \\) for each feature.
2. Evaluate
   \\(f_{-k} := f(x_1, x_2, \dots, x_p) / f(x_1, x_2, \dots, x_{k-1}, \bar x_k, x_{k+1}, \dots, x_p) \\)
3. Decompose the output probability, \\( f(x_1, x_2, \dots, x_p) \\), as
   $$
   \log f(x_1, x_2, \dots, x_p) = bias + \sum_{i=1}^{k} \log f_{-i}.
   $$

The feature with the largest \\( \log f_{-i} \\) value is the most significant.
Moreover contributions can be compared across samples that were given the
same probabilities.

## Technicalities and assumptions

The Naive-Bayes assumption, whereas rarely true in real-life applications,
often works pretty well in practise as it has been shown by Naive Bayes
classifiers. If only approximate contributions are sought after then this
assumption should not be too misleading.

Potentially, the algorithm could be extended to removing 2 and more features at
a time to learn contributions from interaction terms. However the complexity of
such modification grows exponential with the number of interactions considered.

The method assumes that the supplied classifier \\(f\\) is well-calibrated
and outputs correct probabilities.

Definition of a typical value can be tricky, especially in the presence of
missing data. We suggest the typical value should be "missing" if most of the
values are missing for a negative class and should, otherwise, be the mode of
an empirical distribution for the feature.

Defining a mode for continuous range is a bit tricky as it requires
binning values and a hyper parameters emerge for sizes of bins.

### More on evaluating \\(p_{-k}\\)

In order to find

$$
P(y | x_1, x_2, \dots, x_{k-1}, x_{k+1}, \dots, x_p)
$$

formally, one needs to know the density of \\(X_k\\), \\(p(X_k)\\); then

$$
P(y | x_1, x_2, \dots, x_{k-1}, x_{k+1}, \dots, x_p)
=
\int P(y | x_1, x_2, \dots, x_{k-1}, x_k, x_{k+1} \dots, x_p) p(x_k) \; \mathrm{d} x_k
$$

One can estimate \\(p(x_k)\\) empirically by fitting a density function,
\\( \hat p_k(x) \\), into \\( X_k \\), yielding

$$
\int P(y | x_1, x_2, \dots, x_{k-1}, x_k, x_{k+1} \dots, x_p) p(x_k) \; \mathrm{d} x_k
\approx \sum_{i=1}^N f(x_1, x_2, \dots, x_{ki}, \dots x_p) \hat p_k (x_{ki}),
$$

where \\( N \\) is the number of samples in the training dataset, \\(x_{ki}\\) ranges
over the values of \\(X_k\\) in the training data set. Our algorithm approximates
the summation further by plucking out just one term from it where most probability
mass is concentrated, i.e.
\\( \bar x_k := \operatorname{argmax} \hat p_k(x) \\), where argmax is taken over
the range of \\(X_k \\) of the training dataset. Since the dataset is
unbalanced, \\( \bar x_k \\) occurs at the mode of \\( X_k \\) restricted to the
negative class.

## Further work

Finally, evaluation of such feature decomposition on a simulated and real data
is missing for a complete treatment of the subject matter.
