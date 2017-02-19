---
title:  "Surprisingly hard: calling in Python function from R"
tags: R python programming integration
---

For some random side project, I needed to be able to run a piece of Python code from R. Given the two large programmming communities and their focus on being pragmatic, I figures this is probably something that is done often (as to not reinvent the wheel), so there is likely a super-simple solution to this.

Unfortunately I was pretty wrong on that. There seems to be an easy solution to call R from Python, via [rpy](todo), but the other way around turns out to be much harder to do. The packages that promise to do so (rPython, rPythonWin, etc.) are not really widely used and consequently not maintained very well (this is a euphemism for me not being able to set them up, either with a current R version (3.3.2) or the an old one (2.15.1) that they should support, but somehow don't, neither on Windows nor on Linux). This was tiring, and since I really only have to make a few calls to (the same) python function which then does all the work, and runtime performance is not really an option, I then went down another route: 