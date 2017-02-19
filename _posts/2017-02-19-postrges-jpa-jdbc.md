--
title: "PostgreSQL integration testing troubles"
tags: postgresql spring jpa hibernate spring-boot
--

I had quite some trouble this weekend to get a database integration test suite running with current versions of Spring Boot (1.5.1) and PostgreSQL (9.6.1). 

There was this one error that would pop up sporadically:

{%  highlight text %}
org.postgresql.util.PSQLException: ERROR: cached plan must not change result type
{% endhighlight %}

(Actually, I first got just the German[^1] version of that, which did not help either.)

The funny thing was that this error only happened when I ran the full test suite, and it would only happen after about 15 minutes. So much for rapid trial and error.

After testing out various settings (and, different versions of dialects, JDBC drivers, Hibernate versions...), my best guess was that it is not a Spring-related problem; I could not find anything similar to my issue. 

After some more searching for *just* the error message, this [Github comment](https://github.com/molgenis/molgenis/issues/5511#issuecomment-259893998) (unrelated issue, unrelated project) gave me my Heureka-moment: simply appending `?autosave=ALWAYS` to the JDBC URI was all it needed.

In hindsight, there is propbably some side-effect involved in the preceding database tests running in the suite (some also cover failure scenarios), and this led to this strange issue. Further [digging into the code of the Postgres JDBC driver](https://github.com/pgjdbc/pgjdbc/blob/43e6505e3aa16e6acdf08f02f2dd1e3cf131ac3e/pgjdbc/src/main/java/org/postgresql/PGProperty.java#L383) explains what actually happens, and what the options are.

Morale of the story? Append `?autosave=ALWAYS` to your JDBC URI if you are running into the same issue! ;-)[^2]

[^1]: While I like the idea to also translate technical error messages, a translated message is pretty useless without a *language-invariant* error ID: users need to *search* for this term on Stackoverflow etc.

[^2]: And, also, with a more complex system that has many potential culprits for the issue, always track back to the question: *Which part of the setup is most likely the issue?*. I got that wrong for the first few (OK: many) trials, which significantly slowed down the debugging.