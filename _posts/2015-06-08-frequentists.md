---
layout: post
title: Frequentist vs Bayesian statistics and more
visible: 0
---

To demonstrate a difference between Bayesians and Frequentists, I'll use
the following example:

You observe \\(10\\) Heads in \\(14\\) coin flips. Would you bet that in the
next two tosses you will see two heads in a row?

Using a maximum likelihood estimation for probability \\(p\\) that a coin
ends up head, we would say \\(\hat{p} = 10 / 14\\). So probability of
two heads is \\(\hat{p}^2 \approx 0.714^2 \approx 0.51\\). Makes sense to
bet for a frequentist.

*Bayesian approach:* Prior to tossing a coin we knew nothing about it - even if
it was biased we couldn't tell which way it was biased. So let us say \\(p\\)
was drawn from a uniform distribution first. After the tossing - our beliefs
about the coin should change. But how? Bayes rule states

$$ Pr ( p | data ) = \frac{Pr (data | p) Pr (p)}{Pr (data)} $$

Ignore the term \\(Pr (data)\\) since it's the same for all \\(p\\).
\\(Pr (data | p) = {14 \choose 10} p^{10} (1-p)^4\\). Therefore
\\(Pr ( p | data ) \propto p^{10} p^4 Pr (p)\\). \\(Pr (p)\\) is constant
for a uniform \\(p\\) and can be dropped under our proportional
(\\(\propto\\)) notation. And hence *posterior* distribution for \\(p\\)
is \\(Beta(11, 5)\\).

Now it follows that \\(Pr (HH | data) = \mathbb{E} [ p^2 | data ] =
\frac{B(13, 5)}{B(11, 5)} \approx 0.485\\)[^2]. A Bayesian with uniform prior wouldn't
bet.

Notice how \\(Pr\\) is *subjective*. It only exists in the head of a Bayesian.
Frequentists usually treat probability as some intrinsic property of objects
rather than mental tags assigned to beliefs.

Let me restate Bayes theorem:

$$ Pr(Hypothesis | Evidence) = \frac{Pr(Evidence | Hypothesis) Pr(Hypothesis)}{Pr(Evidence)} $$

Just one line. So much for a theorem. Perhaps
most of you have rediscovered it yourself. It probably invoked some kind of
inspiration in you the first time you learned about it and then it became one
of those maths tropes. Boring. Meh. Yawn. ([Not Hot?][notHot])

But! It governs your thought process. Perhaps you don't even realise. Let me
show you just one example out of bazillion. Years ago I had to "resolve" an
argument about the use of English: my friend, who is a non-native speaker, was
trying to correct two native speakers playing a grammar trump card: "Don't you
guys see you are using an adjective in place of an adverb? Why didn't your
schools teach you grammar, guys?". The natives couldn't recall what adverbs
were, but they were sure they were right. "I see where you are coming from, but
you have got two confident natives contradicting you. It must be one of those
pesky exceptions then", - I reasoned. As you probably have guessed, that was
correct.

And the moral of the story? I am always right. Just kidding... almost always
right.

Humour aside - I was originally inclined to believe my friend: her
reasoning *was* right, but the disagreement from the other two participants
changed my mind. The principle of changing your confidence about statements is
at the heart of the Bayes rule. It is called an update. *Prior* to the update I
thought she was right - *after* the update I thought she was wrong.

I have never been taught to put probability in such context. It's usually
coins, urns and balls - but never everyday situations. Yet people are intuitive
Bayesians and I have set out to prove this to you.

Another aspect of Bayesianism is interpreting probability as a degree of belief.
The degree of beliefs is just as natural as Bayesian updates: you just know
some statements are much more certain than others. You might be willing to
bet huge stakes on the validity of many physics laws. Let's take Newton's laws,
for example. Largely quoted to be untrue in general - you would still accept
large bets on falling times of a stone. You know Newton's laws apply here,
hence locally, they are true. Not just truish kind of true, but very very true.
But you probably won't find a person willing to accept a bet with you on the
local validity of the Newton's laws. However, there are many people willing to
trade based on stock market predictions. And you'd be a fool to accept same
odds as you would on Newton's laws. Certainty isn't comparable.

And so this is it: you are a natural born Bayesian. *QED*.

Now that I have spilled the beans, I invite you to become a better Bayesian.

First of all, why? To be more right so that you can win at a gamble called
life. For if the mental tags accompanying your beliefs are at odds with
reality your actions will result in unintended consequences time and time
again.

First baby step: admit that you are a Bayesian. Embrace your inner self.

Further, realise that our intuition is not a good calculator - we often don't
"update" our minds appropriately. In certain situations we are keen to update
in others we decide not to. Psychology has determined one class of situations
where we update *way too much*. This is also known as *base rate* fallacy. The
following example comes from wiki:

John is a man who wears gothic inspired clothing, has long black hair, and
listens to death metal. How likely is it that he is a Christian and how likely
is it that he is a Satanist?

Intuitive answer is that it is likely.

What is a prior in this case? Try "John is a man. How likely is that he is a
Christian and how likely that he is a Satanist." Now you see it: 2 billion
Christians dictate overwhelming probability for the former. That's a prior.
Posterior comes after you learn that he wears gothic clothing. But your belief
should still be dominated by your prior given how many more Christians they are
in the world even if only a small proportion of them wear gothic clothes.
See [wiki][baseRate] for examples with numbers.

The notion of treating likelihood, confidence and probability interchangeably
may cause some confusion in you had you gotten used to the maths education
where frequentist interpretation is the king. I don't know how often
probability is treated as part of logic in undergraduate courses, but in fact
it should be. I am way ahead of you, university curriculum! See the following
quote:

> "The use of probability to represent uncertainty, however, is not an ad-hoc
> choice, but is inevitable if we are to respect common sense while making
> rational coherent inferences. For instance, Cox (1946) showed that if numerical
> values are used to represent degrees of belief, then a simple set of axioms
> encoding common sense properties of such beliefs leads uniquely to a set of
> rules for manipulating degrees of belief that are equivalent to the sum and
> product rules of probability. This provided the first rigorous proof that
> probability theory could be regarded as an extension of Boolean logic to
> situations involving uncertainty (Jaynes, 2003)."
>
> -- <cite> Christopher Bishop. Book "Pattern Recognition and Machine Learning".

So you can call it probably, you can call it confidence, you can call it
betting odds you are willing to accept[^1] - it is all the same: it is your
degree of belief.

I foresee one big objection: in everyday situations \\(Pr(Hypothesis |
Evidence)\\) is often not known. Note that Bayes rule only operates with
subjective probabilities, so given that you have a subjective prior, it's only
natural to allow for subjective updates. Subjective updates from subjective
priors?

If only I could state a maths theorem and calibrate everyone's world view
with reality just like that. However, there is plenty of literature which
highlights most common update errors: this Bayesian approach is a small but
fundamental part of something bigger I simply can't fit into this text.

This brings me to the Bayesian approach applied to maths and stats. We, PhD
mathematicians, daily find ourselves dealing with statements which haven't yet
been proven. And we often use our intuition or intuition of our supervisors
before throwing ourselves at the problems. The other week I looked at a series
and said "No way it converges to a \\(1/2\\)". Reason? Given that there is many
more irrational numbers than rational, only carefully constructed series
converge to a rational. Analogously if you are vehemently trying to prove
\\(P=NP\\) you better read what the world would be like if \\(P\\) was equal
to \\(NP\\) (constructively). And, of course, whilst \\(P \neq NP\\) isn't
proven it could still be either way. But a gambler in me is already willing to
bet. And, on a contrary, if you think that a mathematical theorem is true
because it has been proven... Well, you are neglecting a non-negative chance
that no one has spotted an error just yet. So don't you ever let that \\(Pr\\)
in your head hit 0% or 100%! To be a Bayesian is to always be uncertain of
everything - but to a different degree.

Finally, take hypothesis testing. It deals with the following: given that a
hypothesis is true, how likely it is to observe the data, i.e. \\(Pr(Evidence |
Hypothesis)\\) also known as a p-value. Oh our good old infamous friend
p-value. If a null-hypothesis passes a 5% test, are you 95% certain? Of course
not. The following [xkcd](http://xkcd.com/) comic illustrates it better:

![xkcd](http://imgs.xkcd.com/comics/frequentists_vs_bayesians.png)

The problem with p-values and confidence intervals is that they are not
intuitive. All statistics courses instil not saying statements like "There's a
95% chance that the value is within this confidence interval". Because people
seem to treat p-values as \\(Pr(Hypothesis | Evidence)\\) instead. This
highlights that frequentist approach needs to be taught and Bayesian
interpretation comes naturally. The p-values govern the *updates*, not the
end beliefs and it's right there, in the Bayes rule.

My post must come to an end. But really I have barely started. I highly
recommend ["Map and Territory"][map] sequence of posts by Eliezer Yudkowsky if
you want to know more about updating your beliefs appropriately with
more data. Two particularly relevant blog posts to Bayesianism are:

* [Conservation of Expected Evidence][expected]
* [Absence of Evidence Is Evidence of Absence][absence]

That's it folks. No really, that's it.

[^1]:
     sweeping the technicalities of gambling preferences under the rug...
     Congratulations, you are a pedant!

[^2]: see [Are you a Bayesian or a Frequentist? (Or Bayesian Statistics 101)](http://www.behind-the-enemy-lines.com/2008/01/are-you-bayesian-or-frequentist-or.html) for derivation.

[notHot]: http://chalkdustmagazine.com/category/regulars/whats-hot-and-whats-not/
[baseRate]: http://en.wikipedia.org/wiki/Base_rate_fallacy
[map]: http://wiki.lesswrong.com/wiki/Map_and_Territory
[expected]: http://lesswrong.com/lw/ii/conservation_of_expected_evidence/
[absence]: http://lesswrong.com/lw/ih/absence_of_evidence_is_evidence_of_absence/
