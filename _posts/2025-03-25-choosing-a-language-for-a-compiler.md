---
title:  "Choosing a language for a compiler"
date:   2025-03-25
tags: compiler programming go rust typescript
---

Steve Klabnik[^1] wrote [a nice piece](https://steveklabnik.com/writing/choosing-languages/) on choosing an appropriate language for a software project, in the wake of the kerfuffle regarding [the announcement](https://devblogs.microsoft.com/typescript/typescript-native-port/) by [Anders Hejlsberg](https://en.wikipedia.org/wiki/Anders_Hejlsberg)[^2] that the TypeScript compiler has been re-implemented in [Go](https://en.wikipedia.org/wiki/Go_(programming_language)) for performance reasons. The TLDR of his post is:

> Here’s my opinion on the new typescript compiler:
> You should write programs in the language you want to.

It's a bit disheartening to realize how many developers have _very strong opinions_ for what is essentially a 10x tooling speedup they get for free---you can see for yourself in this [very long GitHub discussion thread](https://github.com/microsoft/typescript-go/discussions/411). The main reason given by the `typescript-go` team is quite interesting because it highlights the peculiarities of _this_ software project, making it clear that the engineering decision was not made lightly and the team was just being economical and _pragmatic_[^3]:

> [...] By far the most important aspect is that we need to keep the new codebase as compatible as possible, both in terms of semantics and in terms of code structure. We expect to maintain both codebases for quite some time going forward. Languages that allow for a structurally similar codebase offer a significant boon for anyone making code changes because we can easily port changes between the two codebases. [...]

Even _this_ sounds like a lot of additional overhead in supporting both the 'normal' TypeScript compiler and new `typescript-go`, so I'm happy the team found a language that would allow for this setup _and_ still give them such a nice speedup.

--

[^1]: One of [the Rust book](https://doc.rust-lang.org/book/) authors and an [early contributor](https://github.com/steveklabnik) to [the Rust language](https://github.com/rust-lang/rust).

[^2]: Of Turbo Pascal, Delphi, C#, and TypeScript fame. He may know a thing or two about programming languages.

[^3]: This reminds me of another favorite programming book of mine, [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/the-pragmatic-programmer-20th-anniversary-edition/). You can download a PDF of an older edition [from archive.org](https://archive.org/details/AndrewHuntDavidThomasThePragmaticProgrammerFromJourneymanToMasterAddisonWesleyLongman2000/).