---
layout: post
title: For the prisoners' dilemma once more
visible: 0
---

## Intro and pay-off matrix

The [Prisoner's dilemma][dilemma] often features in television programmes (such
as Golden Balls) where a couple of contestants have to decide whether they want
to share or steal a pot of money. They make their choices in secret from one
another and then their decisions are simultaneously revealed.

Let the tuple (a, b) mean that you get a and your opponent gets b: so (3, -1)
represents you winning £3 and your opponent losing £1. We introduce the
pay-off matrix for a non-iterated (played only once) prisoner's dilemma:

| You \\(\diagdown\\) Opponent   | Cooperate | Defect  |
|--------------------------------+:---------:+:-------:|
| Cooperate                      | (1, 1)    | (-1, 3) |
| Defect                         | (3, -1)   | (0, 0)  |

Each player is given the opportunity either to defect or to cooperate. In the
original set-up with prisoners the option of defecting meant betraying the
other and testifying whilst cooperation represented remaining silent.

## Nash equilibrium and Pareto efficiency

The definition of Nash equilibrium (NE) implies that if all but one players are
playing at the Nash Equilibrium strategy, the other player is better off
playing it too.

So (Defect, Defect) is a Nash equilibrium strategy since if you know your
opponent defects, you should defect too. (Cooperate, Cooperate) is not NE since
if you know that your opponent cooperates, you should defect.

It is often taught in Game Theory courses that two rational players should
decide to defect as each one is better off defecting no matter what the other
player does. This yields to a paradoxical situation of the players foregoing a
more favourable outcome to both.

Pareto efficiency is defined as a state where it is impossible to make any
one player better off without making at least one contestant worse off. In our
example it clear that (Cooperate, Cooperate) is Pareto efficient, but the
(Defect, Defect) outcome is not.

This leads to one criticism of Nash Equilibrium: the outcome of NE isn't
always Pareto efficient.

Many possible remedies for a more satisfactory solution of the prisoner's
dilemma have been advocated. It is common to make the game played repeatedly to
show that rational players cooperate on the first few rounds but then start
defecting.

A colleague and I stumbled upon the Prisoner's dilemma whilst discussing a
different problem and we originally had a mild disagreement over what the
optimal strategy should be for our scenario. However, after a short
conversation we settled it. Here is how we did it.

## Let the probability guide us

Assume that you (P1) believe that the opponent (P2) cooperates with probability
\\(p_2\\). Your task is to find the probability of cooperating \\(p_1\\),
such that the expected pay-off is maximised. I would like to emphasise that
\\(p_2\\) represents your confidence in the statement that P2 will defect.
\\(p_2\\) is *not* the proportion of times P2 defects if you played the
Prisoner's dilemma many times. This interpretation, also known as Frequentist,
would be problematic for a game played only once. Instead, \\(p_2\\) is simply
your state of knowledge about P2. Such interpretation, Bayesian, will protect
us against [mind projection fallacy][mind], the fallacy when we confuse our
state of knowledge about reality with the reality itself. For example, if we
raise \\(p_2\\) to \\(1\\) does that mean the opponent will cooperate? No, we
just *think* that he will. This will not, telepathically, cause him to
cooperate.

Short foray into plausibility reasoning: if \\(A \implies B\\) and you observe
\\(B\\), does that make \\(A\\) more plausible? Bayes theorem says yes. This
aspect is ignored in classical game theory, but we will rely on it here.

## Finding Nash equilibrium

The strategy assumes that the two players make their decisions independently:
your choice of \\(p_1\\) does not affect what you believe \\(p_2\\) to be.
Hence the expected pay-off is found to be

$$ \mathbb{E}[\text{pay-off}] = p_1 p_2 - p_1 (1 - p_2) + 3 (1-p_1) p_2 $$

which reduces to

$$ \mathbb{E}[\text{pay-off}] = 3p_2 - p_1(1 + p_2) $$

The maximum expected pay-off is always achieved at \\(p_1 = 0\\) for any value
of \\(p_2\\) between \\(0\\) and \\(1\\). So you never cooperate. The
probability set-up is now redundant as your strategy is deterministic. Such a
deterministic strategy is also known as pure. We arrived at the NE strategy
again which is often referred to as rational, yet I'd hesitate to refer to
people who leave the best option on the table, (1, 1), as rational.

## What if you played it with a clone of yourself?

Yeah, that's right. Let's just clone you for the purpose of the game just
before it starts and see how this goes. Because why not. Previously \\(p_2\\)
was independent of \\(p_1\\), but can we now say that your choice of \\(p_1\\)
doesn't change your belief about \\(p_2\\)? The set-up now is as symmetric as
it can possibly get and if you think you are going to cooperate with
probability \\(p_1\\), is there any reason to suppose that *your clone*
cooperates with \\(p_2 \neq p_1\\)? Well... I argue that there isn't. So let's
say that \\(p_1 = p_2 = p\\). Therefore

$$ \mathbb{E}[\text{pay-off}] = p^2 - p(1 - p) + 3 (1-p) p  = -p(p-2)$$

So we have an inverted parabola with roots at \\(0\\) and \\(2\\) readily
maximised at \\(p = 1\\). So you *always* cooperate with your own clone. The
strategy is, again, deterministic. The intuition is also satisfied: if I defect
then the clone defects because of the same reasoning and we both get nothing.
If I cooperate then my clone cooperates and we are both happy. Seems logical
that we both choose to cooperate.

## Back to reality

What if your opponent dresses like you, talks like you, acts like you, but is
not quite you? Let us quantify the "not quite you" part as follows: the
person you are playing with flips a coin in the morning. If it comes up heads
he will defect. It's just one of those days when he's angry at everyone. He
acts just like you would act otherwise. Say the coin has probability of coming
up tails \\(q\\).

$$
p_2 =
\begin{cases}
0, & \text{with probability $1 - q$} \\
p_1, & \text{with probability $q$}
\end{cases}
$$

We call \\(q\\) similarity index: it tells you how similar you think your
opponent is to you. Hence, ignoring the details, \\(\mathbb{E}[p_2] = q p_1\\)
and

$$ \mathbb{E}[\text{pay-off}] = p_1 (3q - 1) \left(1 - \frac{q}{3q - 1} p_1
\right) \,, $$

which is maximised at

$$
p_{\max} =
\begin{cases}
0, & \text{if $q \leq \frac{1}{3}$} \\
\frac{3q - 1}{2 q}, & \text{if $q > \frac{1}{3}$}
\end{cases}
$$

where \\(p_{\max}\\) is dependant on the pay-off matrix. Note that the strategy isn't deterministic for \\(1 / 3 < q <
1\\) and it sits right in-between Nash equilibrium and the one with a
clone. Our intuition is again satisfied: the chance of you cooperating is
determined by the pay-off matrix and by the similarity of the opponent's
thinking process to yours.

I cannot emphasise it enough that even though it seems like P2 telepathically
knows about your strategy, there is not telepathy or magic thinking involved.
The \\(q p\\) strategy is what *you believe* P2 will do and it has nothing to
do with what he will actually do.

## Putting it in perspective

Should your belief about \\(p_2\\) depend on your strategy \\(p_1\\)? I
don't see why it shouldn't. Biological neural networks in our heads run on the
same principles in all of us. So if I convince myself that I should defect then
I have anthropomorphic evidence staring right at me that the opponent
might think to do the same thing. Therefore I should revise my belief about
\\(p_2\\).  Another way to put it: we are all "imperfect" clones of each other.

Let us set up a chain of propositions that could fit this. Let A(X) = "X
proportion of people choose to defect", B = "I end up defecting", C = "My
opponent ends up defecting". Now observing B could raise your estimate of X,
which in turn would raise your confidence in C. Note that there is *no*
physical causality involved: it would be preposterous to reason that your
decision causes your contestant to change his mind. However, there is a logical
causality which governs the probability flow in your mind.

Only when you can verifiably be sure that you are playing against a P2 whose
strategy is completely independent from yours, for example if you are playing
against rolls of a die, you should stick to the Nash equilibrium strategy.

## Different pay-off matrices

We mentioned that \\(p_{\max}\\) was dependent on the pay-off matrix, so let us
play around with our pay-off matrices to check how our solution performs. Keep
(Cooperate, Cooperate) and (Defect, Defect) fixed, but introduce \\((b, c)\\)
instead for (Cooperate, Defect), such that \\(b > 1\\) and \\(c< 0\\).

| You \\(\diagdown\\) Opponent   | Cooperate | Defect  |
|--------------------------------+:---------:+:-------:|
| Cooperate                      | (1, 1)    | (c, b)  |
| Defect                         | (b, c)    | (0, 0)  |

It can now be shown that

$$
p_{\max} =
\begin{cases}
0, & \text{if $q \leq - \frac{c}{b}$} \\
\frac{bq + c}{2(b + c - 1) q}, & \text{if $q > - \frac{c}{b}$}
\end{cases}
$$

Under a technical assumption that \\(b + c \geq 2\\).

You can see that \\(p_{\max} \to 1 /2\\) as \\(b \to \infty\\) for any fixed
\\(q, c\\). Makes sense? You and your contestant are aiming for the \\(b\\)
outcome, but only one of you can have it. Might as well let the randomness
decide who is going to get it. And if \\(c \to -\infty\\) you should stop
cooperating.

## Connection with a cooperative game theory

It is apparent that when one plays against a clone it is equivalent to deciding
upon the strategy beforehand (at time \\(0\\)) and sticking to it throughout
(without the ability to communicate any longer). The field of game theory where
players are permitted to decide upon a strategy first and are obliged to stick
to it is also known as a cooperative game theory. In a cooperative prisoners'
dilemma players can easily achieve the Pareto efficient outcome (C, C) as long
as there is a third party ready to enforce the contract.

In fact, our "back to reality" scenario is equivalent to playing such a
cooperative dilemma only with a person who is somewhat unreliable and feels the
same way about you: then you can both pre-agree that maximising \\(p (3q - 1)
(1 - q p /(3q - 1))\\) is the right thing to do.

So it turns out that allowing plausibility reasoning about your opponent's
strategy based on your own strategy in a classical game theory setting, as we
have done here, leads us to a cooperative game theory. It appears that it is
beneficial for everyone to act as if they signed an implicit contract rather
than out of a selfish self-interest. Perhaps this could explain a number of
social concepts like queueing, voting, not littering, etc.

## What if the game isn't symmetric?

The solution to these kind of dilemmas now easily follows. Imagine that at time
\\(0\\) you could pre-agree with the other players about a strategy, what would
it be?  Just pick the one that leads to a Pareto-efficient outcome and stick
to it.  Hence if you play with clones that's what you should do. We believe
that Pareto-efficiency implies that the strategies \\(s^\*_1, s^\*_2, \dots,
s^\*_N\\) maximise

$$
\min_{1 \leq n \leq N} \mathbb{E}[\text{pay-off for player n}]
$$

because no player would agree to reduce his expectation for the gain of
another player.

Let's work through the "back to reality example", but with different trust
issues. At time \\(0\\) P1 and P2 meet and disclose:

P1: "Let us make an agreement that you cooperate with chance \\(p_2\\), but
I estimate \\((1 - q_1)\\) chance that you will break the agreement and
defect regardless."

P2: "Similarly, I think there is a \\((1 - q_2)\\) chance of you violating
your agreement of cooperating with probability \\(p_1\\)."

So, with the same pay-off matrix as before, let the pay-off for player 1 is

$$
F_1(p_1, p_2) = -p_1 + 3p_2 - p_1 p_2
$$

And the pay-off for player 2, \\(F_2(p_1, p_2)\\), is simply \\(F_1(p_2,
p_1)\\). We can derive the formulas for what values of \\((p_1, p_2)\\) we
achieve the Pareto-efficient outcome for each choice of \\((q_1, q_2)\\):

$$
(p_1^*, p_2^*) = \operatorname{argmax}_{p_1, p_2}
\min \left[ F_1(p_1, q_1 p_2), F_1(p_2, q_2 p_1) \right]
$$

By symmetry, for \\(q_1 = q_2 = q\\), this is achieved at \\(p_2^\* = q
p_1^\*\\). For the asymmetric case solving it numerically yields the following
values:

| \\(q_1, q_2\\)    | 0.75, 1 | 0.5, 1 | 0.25, 1 | 0.1, 1 | 0.75, 0.75 | 0.5, 0.75 | 0.25, 0.75 |
|:-----------------:+:-------:+:------:+:-------:+:------:+:----------:+:---------:+:----------:|
| \\(p_1^\*\\)      | 0.86    | 0.66   | 0.33    | 0      | 0.83       | 0.61      | 0.25       |
| \\(p_2^\*\\)      | 0.99    | 0.93   | 0.67    | 0      | 0.83       | 0.75      | 0.44       |

One interesting choice of \\(q_1, q_2\\) is (\\(0.1, 1\\)) when the
opponent trusts you unconditionally, but knows you don't trust him: in that case
you both are better off defecting then.

In the end, using this method we can find the solution which takes into account
how similar you think you appear to your opponent and how similar you think
your opponent is to you!


## Obligatory references and other titbits

Obviously it turned out that what Stephen and I stumbled upon one evening isn't
new. Douglas Hofstadter introduced the concept of
[superrationality][superrationality] which is equivalent to playing against
yourselves. However, to the best of my knowledge, this interpretation of
rationality is still not widely accepted in the orthodox literature.

And finally, some relevant facts from Golden Balls for your information:

* Individual players on average choose "split" 53 percent of the time.
* There is little evidence that contestants’ propensity to cooperate depends
  positively on the likelihood that their opponent will cooperate (i.e., they
  find little evidence for conditional cooperation).
* Contestants are less likely to cooperate if their opponent has tried to vote
  them off the show in the first two rounds of the game, which is in line with
  the notion that people have an intrinsic preference for reciprocity.

[dilemma]: https://en.wikipedia.org/wiki/Prisoner%27s_dilemma
[regret]: http://lesswrong.com/lw/nc/newcombs_problem_and_regret_of_rationality/
[superrationality]: https://en.wikipedia.org/wiki/Superrationality
[mind]: https://en.wikipedia.org/wiki/Mind_projection_fallacy
