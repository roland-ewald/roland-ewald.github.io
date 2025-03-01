---
title:  "Let's try this again, pt.2"
date:   2025-03-01
tags: meta worklife blogging notetoself links
---

So it turns out I was [wrong on the internet](https://xkcd.com/386/) again, and [the reason I was not blogging more](https://roland-ewald.github.io/2016/12/25/lets-try-again.html) had _nothing_ to do with technology, but more with other tasks eating away my time 😅

I'm not sure how much interesting stuff I will have to talk about, anyway, but to get myself into the habit of writing things here regularly (if only for myself) I will start things off easily and just collect interesting things I've stumbled upon over the last few days.

## An interesting way to shoot yourself in the foot with Java's `TreeMap`

A nice writeup [here](https://josephmate.github.io/2025-02-26-3200p-cpu-util/) (found on [HN](https://news.ycombinator.com/item?id=43207831)), TLDR: concurrent access to objects that are not thread-safe does not necessarily cause exceptions. In data structures like `TreeMap` it may lead to infinite loops, so it looks like a performance issue when troubleshooting.

## Supply chain attack on software delivery company (a near miss)

Fortunately [this vulnerability was apparently found before it was exploited](https://kibty.town/blog/todesktop/) (found on [HN](https://news.ycombinator.com/item?id=43210858)), but otherwise this could have been a total disaster, because [todesktop.com](https://www.todesktop.com/) has some high-profile customers  and such a vulnaribility could result in a "code execution on millions of people", as the author points out. The attack vector is the deployment pipeline:

> [...] with the credentials i have, i could deploy an auto update to any app of my liking, having clients receive it immediately when they restart the app.
