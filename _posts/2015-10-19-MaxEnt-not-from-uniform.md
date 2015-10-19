---
layout: post
title: MaxEnt applies even if you don't start with a uniform prior
---

E.T. Jaynes has a very nice derivation in his book: "Probability Theory: The
Logic of Science", Chapter 9, which allows one to answer the following
question:

After rolling a die many times it transpires that the expectation is \\(4\\).
What is the probability distribution of such a die?

It turns out that if you start with ignorance knowledge: i.e. all rolls
are independent and each number is equally likely one ought to maximise
the information entropy, \\(\sum - p_j \log p_j\\), subject to the following
constraints:

* \\(\sum_{i=1}^6 p_i = 1\\),
* \\(\sum_{i=1}^6 i * p_i = 4\\).

to arrive at the answer.

It is not immediately clear that this principle still applies even if you don't
start with ignorance knowledge. However, it is easy to incorporate your prior
distribution into MaxEnt algorithm in some situations.

For example, start with a 3-sided die with a prior \\([2/6, 1/6, 3/6]\\). Then
it is the same as starting with 6-sided die with a prior \\([1/6, 1/6, 1/6, 1/6,
1/6, 1/6]\\), where rolls \\(1, 2\\) map to  \\(1\\), \\(3\\) to \\(2\\) and
\\(4, 5, 6\\) to \\(3\\).

So if you start with a prior \\([2/6, 1/6, 3/6]\\) and then learn that the true
average is \\(1.5\\) you can apply MaxEnt algorithm under

$$f_1 * 1 + f_2 * 1 + f_3 * 2 + f_4 * 3 + f_5 * 3 + f_6 * 3 = 1.5 \,,$$

and

$$\sum_{i = 1}^6 f_i = 1$$

and it will give you the posterior distribution, which you can collect
back to \\((p_1, p_2, p_3)  = (f_1 + f_2, f_3, f_4 + f_5 + f_6)\\).

This gave me an idea why Shannon's theorem[^1] stipulates the following desired
property of an entropy \\(H\\):

$$H_3(p_1, p_2, p_3) = H_2(p_1, q) + q H_2(\frac{p_2}{q}, \frac{p_3}{q})$$

[^1]:
    Shannon's theorem states desired criteria for a to-be-constructed entropy
    function and the shows that \\(\sum_i - p_i \log p_i\\) is the only function
    that satisfies it.
