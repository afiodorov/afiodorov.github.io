---
layout: post
title: Plausibility = Just Double | DoNotKnow
visible: 1
---

Recently I gave a [talk][talk] on "Probability theory as an extension of logic"
where I stated the [Cox's theorem][cox].

The first assumption of the theorem is that plausibilities are real numbers.

The following objection to the first assumption was raised:

> Why should the plausibility be a real number rather than just "I don't know"
> --- <cite>Olly</cite>

Such idea hasn't been ignored by the people who developed this theory, but
let us first remind ourselves of the purpose of the exercise: *to process the
incomplete information optimally*.

Hence, if you suspect that you don't have enough information for a decision
to be made **and** you can postpone making the decision than spare your brain
from unnecessary computations.

Another way to put it: [garbage in, garbage out][gigo].

But what if your decision can't wait?

So you have a tough decision to make. The deadline is looming and you
decided to get a robot to help you.  And so let us pretend that we managed to
develop the thinking robot based on the principles of Cox's theorem with the
modified first axiom. We also taught it to process English, tapped into your
brain and uploaded your life experience. And after such expensive exercise, you
finally ask the robot the question: what should I do, Mr. Robot?  And the robot
goes ![loading]({{ site.baseurl }}/images/loading_r.gif) for 6 hours and then spits out
DoNotKnow. And so you are left with making the decision yourself. Useless.

### "I don't know" is implied

The reason why we resort to the use of probability theory in reasoning is
because *we don't know*. You give the information to the robot and wait for
its judgement. If it comes out with \\(p = 0\\) or \\(p = 1\\) it means that
you *know*. There is no uncertainty: it's just a matter of chaining logical
steps together.  Once \\(0 < p < 1\\) then you don't know[^prob].

### Refusal to verbalise probability assignments is just assigning them implicitly

If you say that the universe didn't give you enough information to do action
A so you choose to do B it *has* given you enough information.

Day after day we are taking the plunge and actually do something.  Whether it's
a decision to cycle to work or the refusal to do so, whether it's a decision to
join effective altruism movement or a refusal to do so. Whether it's a decision
to stop eating meat or a refusal to do so. There is no such thing as
"unresolved issue". Once you are aware of the issue but choose to ignore it you
*resolved* it.

So the criticism of the first assumption boils down the criticism of forcing
your resolutions to be explicit rather than implicit.

### Further readings

Having to pick from "I don't know" or a real number is somewhat akin to
using a two-dimensional theory, where second dimension is just a boolean flag.

The idea of having a two-dimensional theory for plausibilities hasn't been
ignored, but the results of a simpler, one-dimensional theory speak for
themselves. The proponents of two-dimensional theories haven't yet demonstrated
practical advantage of such framework.

But for a further discussion on the necessity of two-dimensional theories,
look at [A guide to Cox's Theorem][rcox] pages 4-7.

### Title explained

{% highlight haskell %}
data Plausibility = Just Double | DoNotKnow
{% endhighlight %}

This is just a haskell-way of saying that plausibility of a statement is
either a real number or "I don't know".

[^prob]: In Haskell speak: welcome to the "DotNotKnow" [monad][monad]. There is *no* way out!
[monad]: https://wiki.haskell.org/Monad
[talk]: http://afiodorov.github.io/2015/10/17/talks/
[cox]:  https://en.wikipedia.org/wiki/Cox%27s_theorem
[gigo]: https://en.wikipedia.org/wiki/Garbage_in,_garbage_out
[rcox]: http://ksvanhorn.com/bayes/Papers/rcox.pdf
