---
layout: post
title: Causality, wisdom of the crowd and scary thoughts
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

$$P(X_2 | X_1) = P(X_2).$$

So, knowing the value of the first experiment doesn't change the probability
of the second experiment.

However, this is where the way you perceive probability starts to *matter*.

# I. Orthodox statistics

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
between individual guesses. So mutterings of the first respondent changes the
minds of the respondents afterwards. Nonsense as there is no such thing as
telepathy. (*No* communication between respondents is assumed.)

Therefore, in orthodox statistics, individual guesses are independent.

This conclusion is absurd: if the respondents keep overestimating the Emperor's
height, you should start taking this data into account: as it is likely that
the next guess will also be over. But not in orthodox stats, where
independence is the same as "no physical cause".

# II. Physical causality contrasted with logical causality

Bayesian dependence is also known as logical causality. Examples, where
orthodoxy treats events are independent, but Bayesians treat them as dependent,
include:

### Coin tosses

Orthodoxy: probability is a property of a coin, tossing a coin once won't
change the coin (unless you damage it), so first toss tells us nothing about
the outcome of the second toss.

Bayesians: probability is in the head, so the first toss will reveal to me
useful information about the propensity of a coin to come up heads and I will
adjust my probability with each toss.

### Future events

Orthodoxy: probability of a die doesn't change even if I am told about the
future outcomes, because probability is a property of a die, and unless you
deform the die, nothing can change.

Bayesians: probability is in the mind, I can condition on future events and
this will change the way I predict present events. No axioms of probability
theory refer to time at all, so there's no reason to not do it.

### Responses on a survey

Orthodoxy: individual responses don't affect each other as long as people
are not allowed to communicate since telepathy doesn't exist.

Bayesians: my ability to predict a response of the next person improves
with more data. If I detected a systematic error in the previous responses,
it is likely due to some underlying cause, such as folklore/media, this
error is likely to be present in the future responses.

Therefore, the solution to the Emperor's paradox follows:

> The absurdity of the conclusion tells us rather forcefully that the
> \\(\sqrt{N}\\) rule is not always valid, even when the separate data values
> are causally independent; it is essential that they be logically independent.
> In this case, we know that the vast majority of the inhabitants of China have
> never seen the Emperor; yet they have been discussing the Emperor among
> themselves, and some kind of mental image of him has evolved as folklore.
> Then, knowledge of the answer given by one does tell us something about the
> answer likely to be given by another, so they are not logically independent.
> Indeed, folklore has almost surely generated a systematic error, which
> survives the averaging; thus the above estimate would tell us something about
> the folklore, but almost nothing about the Emperor.
>
> -- <cite>E.T. Jaynes</cite>

### Game theory: Prisoners' dilemma

Orthodoxy: my decision is physically independent of my opponent's decision as
there is no telepathy, therefore I should just defect regardless of anything
else.

Bayesians: my decision is anthropomorphic evidence of how humans behave in
this situation, so the way I model the opponent depends on what I decide to
do. So there are situations when [I shouldn't defect][prisoners], since my
cooperation makes it more likely that a similarly-thinking opponent would also
cooperate.

### Game theory: [Newcomb's paradox][newcomb]

Orthodoxy: my decision physically can not affect the prediction, therefore
taking both boxes is the optimal thing to do.

Bayesians: my decision provides me with some evidence on the prediction,
therefore I should sometimes one-box with a credible predictor.

# III. Scary thoughts, finally

If you subscribe to a Bayesian point of view, and chances are you do as people
don't bother reading what they disagree with in the first place, you now have
all-encompassing, timeless causality governing your every day life.

Every time you procrastinate doing X, chances are, you will procrastinate doing
X again in the future. Arguing with this is not taking evidence into the
account. So you only thought your distraction was going to take a minute...
Boom now months of your **future** life are being wasted. That's if you don't
break out of the bad habits. However, every time you give in you are making it
less likely that you ever will.

If you are a smoker and you pledge that this will be your *last* cigarette,
chances are it won't be. And it won't be your last bet, if you are a gambler.

You remember [Roko basilisk][roko]?

If you don't, there's a short summary. Somebody "clever" posted this on
[LW][LW]: if you believe that strong AI will be developed within your lifespan,
that AI might decide to torture all the people who were aware of such outcome
but didn't contribute enough to bring its development forward. Because a delay
in AI development costs millions of lives (strong Friendly AI is likely to
discover immortality). So now all of you, readers, should go and dedicate your
every living moment facilitating AI development.

Yawn. But then Eliezer Yudkowski freaked out and deleted this whole affair and
banned such discussions on LW all together.

When I first heard about such reaction I scoffed at it. With timeless causality
it makes sense, though. If you managed to convince a person to believe in some
kind of version of Roko's basilisk than he'll live in fear and misery, slaving
away everyday. Arguably, some religions already do just that: promise some kind
of after life. But we are talking about really "smart" individuals here slaving
away because of the timeless causality.

So if you already subscribe to timeless causality philosophy, you might want to
be careful about what new pieces of knowledge you want to acquire...


[prisoners]: http://chalkdustmagazine.com/features/breaking-out-of-the-prisoners-dilemma/
[wisdom]: https://en.wikipedia.org/wiki/Wisdom_of_the_crowd
[newcomb]: http://lesswrong.com/lw/nc/newcombs_problem_and_regret_of_rationality/
[roko]: https://en.wikipedia.org/wiki/LessWrong#Roko.27s_basilisk
[LW]: https://wiki.lesswrong.com/wiki/Sequences
