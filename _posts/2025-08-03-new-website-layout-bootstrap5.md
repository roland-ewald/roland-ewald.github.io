---
title:  "Well, that escalated quickly..."
date:   2025-08-03
tags: programming web maintenance ai refactoring
---

What started with me wanting to tweak my [projects page](/projects/), so that it is easier to distinguish old stuff from ongoing projects, ended up with me realizing that this site still uses [jQuery](https://jquery.com/) and Bootstrap 3 (which was EOL'ed in 2019). So, instead I ended up migrating away from this old and trusted combo, towards the shiny 'new'[^bs5] [Bootstrap](https://getbootstrap.com/) 5, which does not rely on jQuery anymore.

## AI to the rescue?

Software maintenance, in the sense of updating depenencies, seems to be one of those fields that should greatly benefit from applying LLMs, as much of the work is some sort of translation anyhow, and there should be sufficient training data when working with extremely popular tools like Bootstrap.

When I started with this task, my intuition seemed right, in the sense that I could give a chatbot a detailed list of CSS classes and Bootstrap features (like components and styled HTML elements) the site is using, and the LLM[^gemini] was able to generate some kind of '_personalized release note_' that spanned both major version updates (bootstrap 3 to 4, and then bootstrap 4 to 5) and _only_ contained changes that I needed to be aware of: for example, the support of the [flexbox](https://www.w3schools.com/css/css3_flexbox.asp) layout 'behind the scenes', or how responsive layouts work now.

However, when it came to some more mundane tasks, like applying the same kind of change to all HTML elements in a given structure, I also turned to AI[^copilot], and this did not work at all: the files were a bit too large and the LLM was too focused on only fixing a very narrow example, not _all_ instances that needed fixing (so in those cases just running a simple search&replace would have been more effective). Worse, it started to switch _content_ around, i.e. it introduced errors without any reason. Clearly, there is still some way to go before we can use these tools as "semi-smart refactoring tools" on files that are not trivially short.

Bottom line: I had to do these boring changes myself, but at least I could drop some very outdated tools and some things a marginally nicer now. Still, I did not yet manage to improve the projects page... 🥲

--

[^bs5]: The initial Bootstrap 5 release was back in May 2021.

[^gemini]: I used [Gemini](https://gemini.google.com) for this, with one of my self-defined 'Gems' to keep the answers concise and include links to the original sources.

[^copilot]: Here I used [CoPilot](https://github.com/features/copilot), which is well-integrated into VSCode.