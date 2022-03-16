---
layout: post
category : Introduction
tags : [Hannah Fry, reductioni]
---

Recently I attended a talk where [Hannah Fry][fry] mentioned [The Wisdom of
Crowds][wisdom] in the following context: she twitted a picture of a jar
with sweets and offered her followers to guess how many sweets are in the jar.
The average of guesses turned out to be almost spot-on, somewhat like 117.9,
whilst the true number is 117.

Call the quantity to be estimated \\( \mu \\). If we were to model each guess
as an independent sample from some random variable \\( X \\)[^3], then we know that
the average has to converge to the expectation of \\( X \\), call it \\( E[X]
\\), by the [Strong Law Of Large Numbers][law].

In the context of Hannah's talk, the phenomena was mentioned to highlight that
aggregately people are predictable. And the stress is on the fact that
\\( E[X] \approx \mu \\).

However, I fail to see the significance of the Wisdom of the Crowds presented
unless the conditions when \\( E[X] \approx \mu \\) are mentioned.  Obviously
it is not hard to make sure that public gets it really wrong. Try integer
factorisation[^2] or any NP-complete (i.e. hard) problem[^1].  You don't have
to go all the way to NP-complete spectrum of problems, there is most definitely
plenty of easy problems where the average guess is really bad.

Therefore I really need to know when the public has a fair chance to get it
right. This is a simple matter of [Making Beliefs Pay Rent (in Anticipated
Experience)][rent]. The phenomena, as presented, has no predictive power and
does not generate testable hypotheses.

So it wouldn't have changed anything had the average guess for the sweets been
within a reasonable margin rather than spot-on.

In conclusion, I insist that people who talk about The Wisdom shift focus
from the fact that the crowds can be wise to *when* they are wise.

[^1]: For the same reason the idea that the public could self-organise
    so that the society is operating at some kind of stationary level (like in
    pure capitalism) makes me cringe because that is short of implying N=NP

[^2]: If only I could break RSA by asking people around...

[^3]: Here do not treat \\( X \\) as a random variable assigned to each person
    rather it is a RV which picks a person at random and each person represents
    a guess so that sampling comes from identically distributed random
    variables.

[fry]: http://www.hannahfry.co.uk/
[wisdom]: http://en.wikipedia.org/wiki/The_Wisdom_of_Crowds
[law]: http://en.wikipedia.org/wiki/Law_of_large_numbers#Strong_law
[reduct]: http://wiki.lesswrong.com/wiki/Reductionism_%28sequence%29
[rent]: http://lesswrong.com/lw/i3/making_beliefs_pay_rent_in_anticipated_experiences/
