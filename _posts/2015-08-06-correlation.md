---
layout: post
title: If correlation does not imply causation, what does?
visible: 1
---

![Compulsory xkcd](https://imgs.xkcd.com/comics/correlation.png)

Right, off I go. If you are a mathematician/statistician/some other kind of
badass and haven't heard of [Cox's theorem][Cox]: go and learn it *now*. I
would highly recommend reading the first two chapters of "Probability Theory:
The Logic of Science" [book][book] by E.T. Jaynes rather than wiki though. Not
convinced? [Stephen Muirhead][stephen]'s quote: "Such a good book! Something I
should have read ages ago". I am sure that if you happen to know Stephen no
more convincing is necessary. Bad luck otherwise.

I can't praise the book highly enough though.  I am surprised that the gist of
it isn't taught at undergraduate level in maths courses. So if you are a
mathematician: read the first 2 chapters. If you are a probabilist than read
all the relevant parts. If you are a statistician/data scientist: first half is
a must. That's a rule of a thumb I've just come up with. You're welcome.

So what is it that I am trying to get you to read? It's the fact that from 3
simple postulates one can derive a field known as plausibility reasoning. All
mathematicians are introduced into Aristotelian logic yet not all are to
plausibility reasoning, which is an extension of it. It's a field of reasoning
which deals with partial information. It is also much more intuitive than
Aristotelian logic (which is a special case of it). We are used to saying that
if \\(A \implies B\\) and \\(B\\) is true nothing can be said about \\(A\\).
But in real life we are tempted to conclude that \\(A\\) is more likely to
occur if we observe \\(B\\). And this is the everyday definition of the word
imply: to make something more likely. I will stick to this definition in the
text below and refer to \\( \implies \\) as deductive implication or
\\(\text{imply}_d\\).

And so we have got this saying: "correlation doesn't imply causality". And
it's true in Aristotelian logic when the definition of the word imply is
deductive. But we, mere mortals, scientists and statisticians have to resort
to the other imply: the imply in plausibility reasoning. Let's examine what it
leads us to.

A new study comes out: "eating asparagus correlates with cancer". You see
a reference to it in the Daily Mail. [Kill or cure][kill-or-cure] keeps a long
list of things Daily Mail reports that cause or that prevent cancer. Apparently,
many things do both. You yawn and ignore it: "correlation doesn't imply
causation". Feels justified. And it is, but not because of the oft-reapeated
adage. It is because there's an alternative hypothesis to a causal link: Daily
Mail being sensational again is more likely. In fact, nothing written on the
pages of Daily Mail about cancer can possibly convince you at this stage. The
prior probability that Daily Mail is just too incompetent is too high. Such a
case where new data can't possibly convince a person to change his mind is
covered in E.T. Jaynes' book, in Chapter 5 "Queer uses of probability theory".

On the other hand, let's suppose you know a thing or two about asparagus and
cancer. May be there's a molecule X in asparagus that has been linked to cancer
in a number of credible studies. And here you read it again that X is up to no
good. Your alternative hypothesis to the causal link is that X doesn't cause
cancer. Yes, of course it's Daily Mail, but you follow it up (you wouldn't
normally). And you raise your confidence in the causal hypothesis. Why?
Because correlation is evidence for causation and counterevidence against
lack of causation. So the posterior odds
\\( \frac{\mathbb{P(\text{causality} | \text{correlation})}}
{\mathbb{P(\neg\text{causality} | \text{correlation})}} \\)
have increased. End result is different this time: you are more convinced in
the causality.

The fact that same new knowledge can result in different beliefs is covered by
E.T. Jaynes in Chapter 6: Elementary parameter estimation. It highlights the
importance of prior knowledge.

In the second case the adage about causality doesn't seem to apply. The fact is
whether a correlation implies a causal effect is highly dependant on your
alternative hypothesis. In many many cases, a causal link is the most
plausible explanation. Which can only be shown via correlations. And so, in
general, the adage is *wrong*. But here's the worrying bit: we insist on
spreading the adage further without explaining the above subtleties.

My preliminary fix would be amending the saying like so: "correlation doesn't
imply causation *when there's an alternative explanation*". The fix is needed
because I dislike [fully general counter arguments][counter]. They empower
people with an ability to reject *inconvenient* truths: "Oh look, they are
trying to prove me wrong! Nice try, but correlation doesn't imply causation
anyway." The emphasis on the *alternative* is crucial, I tried explaining it
above, but consult Chapter 4: Elementary Hypothesis Testing if it wasn't clear:
it has all the missing details, including actual calculations.

How could my fix help? The rejecter of the claim is now obliged to propose that
there's something hidden going on. It could very well be that she is sceptical
of the source or that she believes that an alternative explanation will soon be
found. Such rejections are more useful (and could easily be valid): they
indicate that there is no productive resolution to the disagreement at the
moment.  Providing more and more evidence at this very stage would be an
exercise in futility. However, one must bear "[absence of evidence is evidence
of absence][absence]" in mind. As time goes on and there's no sensible
alternative on the horizon, one must accept that the probability of the causal
link is increasing...

By the way, the alternative text on the xkcd comic which I used as a tagline is
the following: "Correlation doesn't \\(\text{imply}_d\\) causation, but it does
waggle its eyebrows suggestively and gesture furtively while mouthing 'look
over there'."

I hope this has been interesting and you are curious about E. T. Jaynes's book
now. Bye for now!

[Cox]: https://en.wikipedia.org/wiki/Cox%27s_theorem
[book]: http://bayes.wustl.edu/etj/prob/book.pdf
[stephen]: http://www.ucl.ac.uk/~ucahmui/
[kill-or-cure]: http://kill-or-cure.herokuapp.com/
[counter]: http://wiki.lesswrong.com/wiki/Fully_general_counterargument
[absence]: http://lesswrong.com/lw/ih/absence_of_evidence_is_evidence_of_absence/
