---
layout: post
title: Gihub pioneered new way of collaboration (not necessarily in programming)
---

## Preliminaries or "What the hell is Github anyway?"
Github is the most successful website which hosts projects that use git as a
version control (this blog is an example of such project).

Some background for people who don't know what git is (and no, it's not me):
it is a *distributed* version control system. Distributed means there is no
central repository (storage). Each participant has a .git folder in their
project's folder which contains the history of the project. Time to time
participants synchronise their .git folders with each other in order to
communicate changes.

Git is mostly used for open-source software development, but some use it for
science projects (latex) or blogs. It could be used with anything which needs
version controlling, e.g.  files which store progress of games. I sometimes
start a git repository before mass-renaming files just so that I can restore
them if my attempts fail.

I have already explained what distributed means, but haven't empathised that it
implies no other participants are needed at all. In fact anyone can start
using git even with no internet connection. It is just a way to maintain
history of anything that you are working on, somewhat like making multiple
copies but more advanced (git allows for non-linear history tree, visualising
changes, searching through a history tree, reapplying changes from branches or
just deleting changes selectively in the past and much more).

Now there is a subtle point with regards to Github. Since there is no central
repository what is the role of Github? Github plays a role of a participant who
never contributes. His sole role is to always be online and to share his .git
folder with the rest of the world. People can "push" to Github's .git folder
and others can "pull" from it and thus they keep in sync. The only reason
Gihub's .git folder might appear central is because other participants treat it
as such. However all project participants have the entire history of the
project in their respective .git folders and can choose to stop synchronising
for good (thus abandoning Github altogether).

## What is so innovative about Github?

Github's growth in popularity resulted in hosting many popular projects. This
list is incredibly long, but you might be surprised to learn that The
Guardian's frontend is hosted on [there][guardian]. The most incredible thing
about Github is that anything which is hosted there is open for contributions
from anyone in the world with no permission required to be obtained first. The
way I used to think about contribution:

*Consumer*: Hello, I like your software project/magazine/blog and I would like
to add some content. Please tell me how if it's possible.

*The guy in charge*: Sure. I am attaching the rules your piece of work must
follow and please get back to us with the completed work. We will review it and
hopefully include it in our project.

Now, none of the above is necessary for people who use Github instead. Each
user on Gihub can "copy" any project hosted on Github, by pressing the "Fork"
button:

![Fork](https://help.github.com/assets/images/help/repository/fork_button.jpg)

So say your github username is X and you really really like this blog and
want to add an entry. OK, go to [this blog's github page][page] and click
"Fork". Boom, you have the exact copy of my blog under your name X. And now you
can start playing around with it: correct my poor grammar, write that I am a
moron on every page, change that logo above to a monkey or just add a new
entry. Do whatever you want, I don't need to approve of what you do or even
know that you exist - it will be posted in *your* name anyway.

Only when you are done with playing around you can finally contact me
and say: "Hey, Tom, look at what I have done with your blog. Fancy pulling
from my repository to yours?". And then I will look at the changes and if I
like them I will indeed pull, our .git folders will be in sync and your changes
will be accepted ("into the mainstream").

The difference is subtle, but it is there: anyone who knows how to use git
can start working on anything hosted on Gihub right away: no need to ask
for permission or guidance beforehand. And if the original projects ceases
to exist, the forks inherits the original project and becomes mainstream.
In fact some forks become more popular than the original even before the
original project dies: people start forking the forked project instead because
they like the fork more.

As an added bonus Github forms a completely transparent social network which is
completely meritocratic since contributions of each member are public. So join
us and have fun forking!

[guardian]: https://github.com/guardian
[page]: https://github.com/afiodorov/afiodorov.github.io
