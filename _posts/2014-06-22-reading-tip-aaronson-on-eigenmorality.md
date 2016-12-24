---
layout: blogpost
title: "Reading tip: Scott Aaronson on how to resolve circular philosophical definitions"
date: 2014-06-22
tags: reading-tips philosophy linear-algebra game-theory page-rank
---

A few days ago, [Scott Aaronson][1] posted a very interesting piece on how to [avoid circular philosophical definitions with linear algebra][2].

The basic idea is to apply a variant of Google's [PageRank][6] algorithm (which defines a page as important if other important pages link to it). 

There's also a [paper][4] and [some code][3] by one of his students, Tyler Singer-Clark, who tested out the idea by estimating the morality of agents playing a tournament of [iterated prisoner's dilemma][5].

[1]: https://en.wikipedia.org/wiki/Scott_Aaronson
[2]: http://www.scottaaronson.com/blog/?p=1820
[3]: https://github.com/tscizzle/IPD_Morality
[4]: http://www.scottaaronson.com/morality.pdf
[5]: https://en.wikipedia.org/wiki/Iterated_prisoner%27s_dilemma
[6]: https://en.wikipedia.org/wiki/Pagerank