---
layout: blogpost
title: "Spring Integration Testing: JPA Populators vs. Hibernate Import"
date: 2014-06-29
tags: spring java testing jpa hibernate best-practice
---

[Spring][1] sometimes makes it hard to follow best practices. In particular, new features may not be production-ready yet, or lack functionality that is crucial for the given situation.

Due to Spring's long history,[^1] there is also much outdated documentation (like this post, a year from now ;-) while the documentation of current features may still be incomplete.[^2] This makes it hard to decide when to use a Spring feature, and when to skip it.

One example for this are [Spring Data JPA][4]'s [repository populators][6], which allow to initialize a [JPA][13] data source from a JSON file.[^3] This is particularly useful for integration testing and has several advantages, for example:

* Maintainability, because JSON is well-known and typically easy to read.

* Independence of the underlying database technology (as the populators are based on JPA).

* Independence of the JPA implementation (haven't tested that yet).

__*However*__, here's the catch: you will [need some extra code][2] to support the definition of object references! Object references typically end up as foreign keys in the database, so many non-trivial database schemata are *not supported* out of the box.

At the same time, [Hibernate][7] allows you to [specify SQL scripts for database initialization][8] via its `hibernate.hbm2ddl.import_files` configuration file property. This solution seems less general, but good enough for setting up test databases:

* SQL is a well-known language, too, so the SQL scripts should be as easy to maintain as a JSON-based initialization.

* Initialization scripts for test databases will typically consist of `INSERT` statements only, which should be easy to port to other SQL dialects.

* It seems unlikely that the test database technology changes frequently, as one would simply stick with some pure-Java solution like [H2][10] or [HSQLDB][11].

* Importing SQL on startup is supported by other JPA implementations as well, although it may require extra effort (e.g. see [these instructions][9] for [EclipseLink][12]).

On top of that, SQL is much more powerful: in case you also need to include views, stored procedures, or triggers, you *don't* have to adjust any code for database initialization.

__TL;DR: Use [JPA populators with JSON files][6] to initialize simple (single table) test databases, otherwise let your JPA implementation import SQL files instead.__


------


[^1]: Spring 1.0 was released [more than a decade ago][3]
[^2]: This is not a critique of Spring as such; its [reference manual][5] is up to date and quite good overall (the only problem might be the lack of a commenting function).
[^3]: XML files are supported too.

[1]: http://spring.io
[2]: http://www.sureshpw.com/2014/05/importing-json-with-references-into.html
[3]: https://spring.io/blog/2004/03/24/spring-framework-1-0-final-released
[4]: http://projects.spring.io/spring-data-jpa/
[5]: http://docs.spring.io/spring/docs/4.0.5.RELEASE/spring-framework-reference/htmlsingle/
[6]: http://docs.spring.io/spring-data/jpa/docs/1.6.0.RELEASE/reference/html/repositories.html#core.repository-populators
[7]: http://hibernate.org/
[8]: http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch03.html#configuration-optional
[9]: http://stackoverflow.com/a/2731513/109942
[10]: http://www.h2database.com/
[11]: http://hsqldb.org/
[12]: https://www.eclipse.org/eclipselink/
[13]: http://en.wikipedia.org/wiki/Java_Persistence_API