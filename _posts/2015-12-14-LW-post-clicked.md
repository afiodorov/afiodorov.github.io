---
layout: post
title: Why do we not consult statisticians about everything?
---

> If, outside of their specialist field, some particular scientist is
> just as susceptible as anyone else to wacky ideas, then they probably
> never did understand why the scientific rules work.  Maybe they can
> parrot back a bit of Popperian falsificationism; but they don't
> understand on a deep level, the algebraic level of probability theory,
> the causal level of cognition-as-machinery. They've been trained to
> behave a certain way in the laboratory, but they don't like to be
> constrained by evidence; when they go home, they take off the lab coat
> and relax with some comfortable nonsense.  And yes, that does make me
> wonder if I can trust that scientist's opinions even in their own
> field - especially when it comes to any controversial issue, any open
> question, anything that isn't already nailed down by massive evidence
> and social convention.
>
> --- <cite>Eliezer Yudkowski, [Outside the Laboratory][out]</cite>

The above quote describes the moment when my readings of LessWrong's core
sequences *clicked*. Of course, if a job of a statistician is to come up with
empirical truths from observations, they ought to have an advantage over the
rest of us when it comes to everyday opinions.

Because it's statisticians who should be aware of [base rate fallacy][base],
[Berkson's paradox][berk], [inspection paradox][inspection], [Simpsons's
paradox][simpson], [Will Rogers's paradox][rogers] and many others.

It is statisticians who should've internalised [Occam's razor][razor] as they
use it often in a form of [AIC][AIC] or [BIC][BIC] test.

And it's statisticians who have explicitly been taught the most useful
epistimology there is at the moment (Bayesian interpretation of probability,
referred to as X):

> “Epistemology X” is the synthesis of Aristotelianism and Anton-Wilsonism[^1].
> It concedes that you are not certain of any of your beliefs. But it also
> concedes that you are not in a position of global doubt, and that you can
> update your beliefs using evidence.
>
> ...
>
> Epistemology X is both philosophically superior to its predecessors, in that
> it understands that you are neither completely omniscient nor completely
> nescient; instead, all knowledge is partial knowledge. And it is practically
> superior, in that it allows for the quantification of belief and therefore
> can have nice things like calibration testing and prediction markets.
>
> --- <cite>Scott Alexander, [On first looking into Chapman’s “Pop
> Bayesianism”][pop]</cite>

----

So, *why* do we not call our statistician friends when we are not sure about
something?

The reality is quite ugly at the moment. Statisticians and empiricists are, on
the contrary, equipped with superweapons to *reject* any claim. The way
orthodox statistics is taught today it leaves the following impression: it can
be used to justify anything. Don't believe me? Read the next hilarious quote:

> Parapsychologists are constantly protesting that they are playing by all the
> standard scientific rules, and yet their results are being ignored - that
> they are unfairly being held to higher standards than everyone else. I'm
> willing to believe that. It just means that the standard statistical methods
> of science are so weak and flawed as to permit a field of study to sustain
> itself in the complete absence of any subject matter.
>
> --- <cite>Eliezer Yudkowski, see more at [Parapsychology: the control group for science][para]</cite>

The ability of superweapons to reject *anything* is a perfect case study of the
following phenomena, when knowledge of biases can hurt people:

> Once upon a time I tried to tell my mother about the problem of expert
> calibration, saying:  "So when an expert says they're 99% confident, it only
> happens about 70% of the time."  Then there was a pause as, suddenly, I
> realized I was talking to my mother, and I hastily added:  "Of course, you've
> got to make sure to apply that skepticism evenhandedly, including to
> yourself, rather than just using it to argue against anything you disagree
> with—"
>
> And my mother said:  "Are you kidding?  This is great!  I'm going to use it all the time!"
>
> --- <cite>Eliezer Yudkowski, [Knowing about biases can hurt people][hurt]</cite>

--------

And this is why we don't call our statistician friends. Most are too powerful
at justifying any conclusion so they end up justifying a convenient one. Truth
to them ceased to exist: *"all opinions are valid"*, I heard it once *from a
statistician*. From a person whose day-to-day job is to separate out incorrect
opinions from correct ones.

This rings an alarm bell. No, statistics isn't a social convention we do to get
oneself published. And it also makes we wonder whether I can trust *academic
papers* of statisticians who don't see it this way.

---------

Solutions? To emphasise that statistics is a manifestation of the **quest for
truth**.

Such prosaic idea sidetracks aspiring statisticians into philosophy at the very
beginning of the teaching process. However, we tried jumping straight into
maths without worrying about interpretations. Such leap failed, see the
parapsychology example. So we must extract the most useful ideas from
philosophy and teach it in the first weeks of Statistics 1 courses.
Implications of [Cox's theorem][Cox] is a good start followed by a long list
of statistical pitfalls listed above.

Step 1 to fixing science: writing a blog post about it. Check.

[^1]: Scott's summary of it: the most virtuous possible epistemic state is to
      not believe anything. This leads to nihilism, moral relativism,
      postmodernism, and mysticism.  The truth cannot be spoken, because any
      assertion that gets spoken is just another dogma, and dogmas are the
      enemies of truth. Truth is in the process, or is a state of mind, or is
      [insert two hundred pages of mysterianist drivel that never really
      reaches a conclusion].

[out]: http://lesswrong.com/lw/gv/outside_the_laboratory/
[base]: https://en.wikipedia.org/wiki/Base_rate_fallacy
[berk]: https://en.wikipedia.org/wiki/Berkson%27s_paradox
[inspection]: https://en.wikipedia.org/wiki/Renewal_theory#The_inspection_paradox
[simpson]: https://en.wikipedia.org/wiki/Simpson%27s_paradox
[rogers]: https://en.wikipedia.org/wiki/Will_Rogers_phenomenon
[razor]: https://en.wikipedia.org/wiki/Occam%27s_razor
[pop]: http://slatestarcodex.com/2013/08/06/on-first-looking-into-chapmans-pop-bayesianism/
[AIC]: https://en.wikipedia.org/wiki/Akaike_information_criterion
[BIC]: https://en.wikipedia.org/wiki/Bayesian_information_criterion
[para]: http://lesswrong.com/lw/1ib/parapsychology_the_control_group_for_science/
[hurt]: http://lesswrong.com/lw/he/knowing_about_biases_can_hurt_people/
[Cox]: https://en.wikipedia.org/wiki/Cox%27s_theorem
