---
layout: post
title: There is no such thing as a biased coin
category: probability-the-logic-of-science
---

I want to start a mini-series on "Probability Theory: The Logic of Science".
I will try to get to "Causal vs logical dependence".

------

There is nothing, no physical property that you can measure that will tell you
how likely it is that a coin will turn up heads.

Centre of mass is irrelevant, the coin's width, length and whatever is
irrelevant.

Because probability is in *your* head, in your heeeeead. Zoooombies,
zoooombies.

Give me a coin and I will construct a throwing device to approach any frequency
I want.

Newtonian physics is deterministic. Measure the initial state and watch the
coin follow its computable path.

The same can be said about everything else, with an uncomfortable exception for
quantum mechanics. Uncomfortable because some physicists are still thinking
about it: and many, many weren't keen on accepting the quantum effects being
probabilistic[^1].  Anyway, I am out of my depth here, so let's go back to
easy stuff.

E.T. Jaynes spent an evening throwing various coins to demonstrate exactly this
point. He knew physics and managed to "bias" all of them in any way he pleased.

Probability is subjective: it depends on the observer. Some observers could
know more than others. May be they know physics and are capable of calculating
the trajectory. May be they know that the experiment is staged or some might
have gotten a glimpse of a coin just after it landed. Suddenly probability
assignments differ amongst them.

This idea, that probability is built into the physical world around us, that
it's out there to be discovered via a design of clever experiments, via
orthodox statistical tools which were painstakingly developed took science
astray.

But the problem with orthodox methods and the orthodox idea of probability
being something physical kinda works in many situations. And a Bayesian
refinement usually only gets you tiny-little gains in accuracy, so is not worth
it. And that's why this world view hasn't been abandoned. By the cracks are
showing if you push it too much. Not only the methods are capable of producing
absurd conclusions, they are also self-limiting.

"How can you apply Central Limit Theorem to number theory?" - I heard today.

I guess the question makes sense, if you insist on viewing probability as
something physical, something built into this world, something to be discovered
by flicking coins and throwing dice. But numbers aren't like that. You can
just compute them. *But the same goes for a trajectory of a coin*.

This idea of "randomness as a property of an object or a process" is completely
unnecessary. If you don't know the answer - use probability theory. Because
that's what it is for: dealing with insufficient information. Beautifully
derived from the Cox's theorem. See the details in "Probability Theory: The
Logic of Science" by E.T. Jaynes.

Now I reserve the right to introduce "fair coins", "biased dice", etc. in
*pure* maths only. I treat it as a shorthand. It is much more elegant than
saying "let's start with an observer who assigned the following probabilities
to the following events". That's just too much.

But, but, but, but, but... Statisticians must outright taboo such statements.
Because such philosophy corrupted science. The moment you say "assume
measurement errors are normally distributed" - you are doing it *wrong*. Why?
Here's some data:

~~~~~~~~~~
33 7 20 18 45 31 93 52 50 29 11
~~~~~~~~~~

What distribution should you assign to a process producing it? Answer:
ParsingError and exit(1). It is *known* data, presented in front of you, there
is *no* probability distribution to assign to it as there is no uncertainty.

I can't wait for a revolution in modern statistics: because it's inevitable
that such treatment of probability will be banished outright in the future.
In the meantime, such thinking is too ingrained, too established. Because it
kinda works. Thanks, Fisher.

On pragmatic grounds I don't think everything should be derived from Cox's
theorem. It is just an ideal. Even though we haven't proved \\(P \neq NP\\) we
still deploy encryption relying on this assumption. However, what *has* been
derived should be used in favour of what *hasn't* when such choice is
available.

-------

Now isn't this satisfying? There is no need to worry about what "random" is,
how to construct "random" variables and how to deal with the fact that
now matter how hard you try the result is always pseudo random and imperfect
instead.

*To be continued*.

[^1]: I mean Born's rule, not Shrodinger equation.
