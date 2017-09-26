---
title:  "Want to comment? Write an email."
date:   2017-09-26
tags: web performance ads user-tracking disqus
---

I could confirm the results [posted by Ayush Sharma](https://notes.ayushsharma.in/2017/09/im-killing-disqus-comments-on-my-blog-heres-why) (and featured on [HN](https://news.ycombinator.com/item?id=15334207)) regarding the large performance hit a website takes when loading [Disqus](https://disqus.com/) comments, apparently because they collaborate closely with some user-tracking ads company.

This page, for example, loads *10 times slower* when tested with [GTmetrix](https://gtmetrix.com) (not sure how objective this is): `0.7` seconds vs. `7.2` seconds.

This is quite a lot, and the over 70 redirects they generate, e.g. to the following domains, are certainly not helping:

````
https://aa.agkn.com
https://beacon.krxd.net
https://cm.g.doubleclick.net
https://d.agkn.com
https://dpm.demdex.net
https://e.nexac.com
https://ei.rlcdn.com
https://i.liadm.com
https://ib.adnxs.com
https://idsync.rlcdn.com
https://io.narrative.io
https://licensebuttons.net
https://loadus.exelator.com
https://p.adsymptotic.com
https://pippio.com
https://pixel.tapad.com
https://pm.w55c.net
https://rc.rlcdn.com
https://secure.insightexpressai.com
https://sp.adbrn.com
https://stags.bluekai.com
https://staticxx.facebook.com
https://sync.mathtag.com
https://tags.bluekai.com
https://usermatch.krxd.net
https://www.facebook.com
https://x.bidswitch.net
https://x.dlx.addthis.com
````

This is on top of all the privacy and security implications, and since nobody comments here anyway I will just disable them. 

If you absolutely need to tell me I'm wrong, you can just write an e-mail :-)