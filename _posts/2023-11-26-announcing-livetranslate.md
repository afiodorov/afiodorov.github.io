---
layout: post
title: Releasing Livetranslate
---

I am tired of Americanism: consuming in the Anglosphere sometimes makes me feel
like all movies are the same. America is a great country for allowing a massive
quantity of various productions to flourish however due to commercialisation
pressures some creativity is stiffled.

Growing up I enjoyed some Soviet cinema. Most of it was boring however some
films I loved and kept rewatching. However these days I can't find Russian
stuff with dubs/subtitles - so I am forced to watch this stuff alone. And
sometimes I want to share the experience. This led me to consider the
possibilities of leveraging large language models (LLMs) to facilitate
translation for such content.

Current transcription technologies like DeepGram offer decent performance, but
there's an evident bias towards Western languages. When it comes to Russian and
Polish, for instance, inaccuracies are more common. Those inaccuracies could
easily be fixed by an LLM post-processing the result: I experimented with using
an LLM to improve the quality of these transcriptions, but real-time processing
is still beyond our reach due to the response times of models like
GPT-3.5-turbo (let alone GPT-4).

When it comes to the translation, again LLMs are just not fast enough to
translate real-time, so I am sticking with Google Translate. And surely enough
I can just preprocessing most content and translate thing myself - but that's
much more effort than just putting something on.

In the meantime, I've found that automatic translation can be quite useful for
those who have a basic grasp of the source language. It serves as a helpful
tool for understanding and learning, similar to how one might pick up slang
from context within a movie. This has proven especially true for me with Polish
content - all of a sudden I found it understandable (a Russian speaker
typically doesn't understand Polish but can read a lot of it without any
learning).

Given the current limitations of real-time translation, I've decided to release
my project, [LiveTranslate][LiveTranslate], on GitHub. It's a practical solution for now,
particularly for programmers who might be interested in contributing to its
development. The project has potential for further enhancements, especially in
the areas of user interface and increased accuracy.

The repository is now available for access and contribution. This release is
for those who share an interest in making non-English content more accessible
and are looking for a way to integrate programming skills with language
translation. The hope is that as LLMs improve in latency, so too will the
capabilities of LiveTranslate.

[LiveTranslate]: https://github.com/afiodorov/livetranslate
