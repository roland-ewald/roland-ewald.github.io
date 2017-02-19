---
title: "PostgreSQL integration testing troubles"
tags: postgresql spring jpa hibernate spring-boot
---

I had quite some trouble this weekend to get a database integration test suite running with current versions of Spring Boot (1.5.1) and PostgreSQL (9.6.1). 

There was this one error that would pop up sporadically:

{%  highlight text %}
org.postgresql.util.PSQLException: ERROR: cached plan must not change result type
{% endhighlight %}

(Actually, I first got just the German[^1] version of that, which did not help either.)

The funny thing was that this error only happened when I ran the full test suite, and it would only happen after about 15 minutes. So much for rapid trial and error.

After testing out various settings (and different versions of JPA dialects, JDBC drivers, Hibernate versions...), my last guess was that it is not a Spring/JPA/Hibernate-related problem. While each change slightly altered the results (i.e. the number of errors or in which test it would occur), none of the changes really helped.

Finally, after some more searching for *just* the error message, this [Github comment](https://github.com/molgenis/molgenis/issues/5511#issuecomment-259893998) (unrelated issue, unrelated project) gave me my Eureka moment: simply appending `?autosave=ALWAYS` to the JDBC URI was all it needed.

In hindsight, there is propbably some side-effect involved in the preceding database tests running in the suite (they also cover some failure scenarios), and this led to this strange issue. Further [digging into the code of the Postgres JDBC driver](https://github.com/pgjdbc/pgjdbc/blob/43e6505e3aa16e6acdf08f02f2dd1e3cf131ac3e/pgjdbc/src/main/java/org/postgresql/PGProperty.java#L383) explains what actually happens, and what the options are.

Morale of the story? Append `?autosave=ALWAYS` to your JDBC URI if you are running into the same problem! ;-)[^2]


------


[^1]: While I like the idea of translating technical error messages, a translated message is rather useless without a *language-invariant* error ID: users need to *search* for this term!

[^2]: And, also, with a more complex system that has many potential culprits (i.e. debugging 'targets'), always track back to the question *Which part of the setup is most likely the issue?* -- I got that wrong for quite some time, which slowed down the debugging a lot.