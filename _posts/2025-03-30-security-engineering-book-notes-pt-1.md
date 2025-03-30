---
title:  "Notes on 'Security Engineering', part 1"
date:   2025-03-30
tags: security it textbook programming computer-science management networking reading-tips
---

I am currently reading the classic **textbook on _[Security Engineering](https://www.cl.cam.ac.uk/archive/rja14/book.html)_**[^pdf] by the late [Ross Anderson](https://en.wikipedia.org/wiki/Ross_J._Anderson),[^obituary]; here are some personal highlights from part I (eight chapters on the basics, covering everything from psychology to cryptography):

- The abbreviation for **access-control list (ACL)** is pronounced "**ackle**". This reminds me of other well-known technical abbreviations with dedicated English pronounciations, such SQL (pronounced _sequel_, also [for historic reasons](https://en.wikipedia.org/wiki/SQL#History)) and WSDL (_whistle_). 

- **Names are _even harder_ when securing distributed systems**. This reminded me of Patrick McKenzies' (a.k.a. [patio11](https://news.ycombinator.com/user?id=patio11)) classic essay about [invalid assumptions programmers have regarding names](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/). The _Needham naming principles_[^naming], by [Roger Needham](https://en.wikipedia.org/wiki/Roger_Needham), focus on security-relevant aspects of names, for example at which time they are bound to a principal, how they are resolved, or how easy it is to validate them. The book then goes on to add _even more_ problems in the follow-up section, and in particular mentions ["Zoko's triangle"](https://en.wikipedia.org/wiki/Zooko%27s_triangle), formulated here as:

> No naming system can be globally unique, decentralised and human-meaningful.

- Even the basic discussion of cryptography ([chapter 5](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3-ch05.pdf)) gives, besides a very brief theory overview, a lot of practical advice and argues that **most systems are broken by bad defaults and other engineering problems**, like overcomplicated APIs or unforeseen side channels, and not by [cryptanalysis](https://en.wikipedia.org/wiki/Cryptanalysis)[^xkcd-security]:

> Very few attacks on systems nowadays involve cryptanalysis in the sense of a
mathematical attack on the encryption algorithm or key. [...] Most attacks nowadays exploit the implementation.

- I really like the overall ethos of the book: to successfully secure a distributed _system_---which does not just consist of hardware and software, as it is operated by _people_ in a specific _context_---**security engineering needs to to also consider psychology, economics, game theory, and other related sciences**. This is not only relevant for threat modeling, but also for change management or the assessment of new threats, like cryptanalysis breakthroughs. In [chapter 5](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3-ch05.pdf)[^cryptanalysis-assessment], for example, the book reminds us to stay calm when we read that, for example, "[someone broke TLS](https://www.google.com/search?hl=en&q=someone%20broke%20TLS)"[^reasoning]:

> When someone discovers a vulnerability in a cryptographic primitive, it may or may not be relevant to your application. Often it won’t be, but will have been hyped by the media – so you will need to be able to explain clearly to your boss and your customers why it’s not a problem.


- Something I found surprising on a personal level is that apparently **crime is _not_ declining, it is just moving online (and thus becomes invisible in crime stats)**. From [chapter 2](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3-ch02.pdf)[^crime-stats]:

> There is less of that liability dumping now, but the FBI still records much cybercrime as ‘identity theft’ which helps keep it out of the mainstream US crime statistics.

and from [chapter 8](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3-ch08.pdf)[^crime-stats2]:

> There’s very little police action against cybercrime, as they found it simpler to deter people from reporting it. [...] this enabled them to claim that crime was falling for many years even though it was just moving online like everything else.

- The book is also **full of interesting anecdotes**, for example how a senior [GCHQ](https://en.wikipedia.org/wiki/GCHQ) official insisted that _"There’s nothing interesting happening in cryptography, and Her Majesty’s Government would like this state of affairs to continue.”_ [^gchq-anecdote], or how the [Irish police tried to persecute a notoriously bad driver called **Prawo Jazdy**](http://news.bbc.co.uk/2/hi/uk_news/northern_ireland/7899171.stm), which is Polish for 'driving license'[^driving-license].


I'm sure the second part of the book, focusing on more specific topics, will also be a fun read.

--

[^obituary]: [Here](https://www.openrightsgroup.org/blog/tribute-to-ross-anderson/) is a nice obituary that highlights some of his work outside academia.

[^pdf]: The book [is freely available](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf), the last edition is from 2020.

[^naming]: Section 7.4.1 from page 260 in the book (p. 305 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).

[^xkcd-security]: [This classic xkcd comic](https://xkcd.com/538/) makes a similar point :-)

[^cryptanalysis-assessment]: Section 5.3.3 from page 162 in the book (p. 209 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).

[^reasoning]: This point nicely illustrates why we need to look at the whole system: the fact that such discoveries _are_ "hyped by the media" are caused by human psychology (people being scared by something out of their control), by economic incentives (scary stories sell better in an 'attention economy'), and even by the incentives of the security researchers discovering the mathematical attack (they need to deliver impactful research to improve the odds of future funding).

[^crime-stats]: Section 2.3.2 from page 48 in the book (p. 96 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).

[^crime-stats2]: Section 8.6.6 from page 304 in the book (p. 348 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).

[^gchq-anecdote]: See footnote on page 23 in the book (p. 71 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).

[^driving-license]: Section 7.4.1 from page 262 in the book (p. 307 [in the PDF](https://www.cl.cam.ac.uk/archive/rja14/Papers/SEv3.pdf)).