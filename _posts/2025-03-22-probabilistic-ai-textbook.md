---
title:  "New textbook on 'Probabilistic Artificial Intelligence'"
date:   2025-03-22
tags: ai computer-science reinforcement-learning math textbook
---

The [new free textbook](https://arxiv.org/abs/2502.05244)[^1] on _Probabilistic Artificial Intelligence_  by [Andreas Krause](https://scholar.google.com/citations?user=eDHv58AAAAAJ) and [Jonas Hübotter](https://scholar.google.com/citations?user=pxi_RkwAAAAJ) looks really great, both in terms of content[^2] and presentation, with nicely formatted derivations (including, oftentimes, _comments_ for each step in a derivation) and clean pseudo-code for algorithms. It also offers a great variety of side notes that even include additional diagrams. This is quite helpful to build up intuition and improve understanding of the material.

The book reminds me---in all the good ways---of the classic [Elements of Statistical Learning](https://hastie.su.domains/ElemStatLearn/)[^3]. I recommend this to anyone interested in a more formal introduction to 'AI algorithms' that deal with uncertainty.

The one area that where I would have loved some more details is [Causal inference](https://en.wikipedia.org/wiki/Causal_AI)[^4]. Maybe this is something to include in a second edition^^

--

[^1]: Found [via HN](https://news.ycombinator.com/item?id=43318624).

[^2]: I've leaved through the table of contents and checked out the chapters on Markov decision processes and reinforcement learning, which brought back fond memories :-)

[^3]: By Trevor Hastie, Robert Tibshirani, and Jerome Friedman; [freely available online](https://hastie.su.domains/ElemStatLearn/download.html), and with a more hands-on 'sibling textbook', _An Introduction to Statistical Learning_, also freely available [here](https://www.statlearning.com) (even including code samples in R and Python).

[^4]: Here is [a relatively recent survey](https://dl.acm.org/doi/abs/10.1145/3444944) ([PDF](https://dl.acm.org/doi/pdf/10.1145/3444944)) of the field; the approach is interesting for biomedical research, e.g. in genetics (see the discussion [here](https://perspectivesinmedicine.cshlp.org/content/12/3/a041271.full.pdf+html)). For more information, Judea Pearl, who (as far as I know) developed the approach and coined the term, [has written a lot about this topic](https://scholar.google.com/citations?user=bAipNH8AAAAJ).