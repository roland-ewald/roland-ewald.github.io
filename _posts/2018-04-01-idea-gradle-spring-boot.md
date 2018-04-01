---
title:  "Spring Boot projects in IntelliJ IDEA"
date:   2018-04-01
tags: intellij idea gradle spring-boot spring java software-development
---

## Problem

Setting up [gradle-based](https://gradle.org/) [Spring Boot](https://github.com/spring-projects/spring-boot) projects in [IntelliJ IDEA](https://www.jetbrains.com/idea/) is tricky.

A good development experience means it is easy to

- start any `@SpringBootApplication` (via its `main` method)

- start any test case, including integration tests using `@SpringBootTest` etc.

Since local development machines cannot reach all remote services and often rely on mock-up beans for those (which, of course, are part of the `test` code), and also rely on `.properties` files for application profiles such as `test` or `dev` (which, of course, are part of the `test` resources), you may run into trouble when trying to run applications and tests in IDEA.

This is because IDEA [does not allow](https://youtrack.jetbrains.com/issue/IDEA-160167) simple ad-hoc modifications to the classpath, and at the same time will override all manually changed dependencies whenever you touch a `build.gradle` file _and_ will also not honor the convention of adding test code and test resources to the Java classpath[^1].

## Solutions

To solve this, there are several options:

* You can manually configure your IDEA module to also depend on the test sources and all resource directories. This is only feasible when the gradle setup rarely changes, because the dependencies will be reset when the gradle project is updated.

* You can [delegate IDEA build and run tasks to Gradle](http://mrhaki.blogspot.de/2016/11/gradle-goodness-delegate-build-and-run.html), which works but is rather slow.

* You can play around with custom class loaders or [`-Xbootclasspath/a:`](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/java.html), and the like. However, this is also tricky and makes the whole setup more fragile.

* You can play around with the raw XML that define the IDEA modules and add dependencies there (or use the `idea` gradle plugin's  [`ẁhenMerged`](https://docs.gradle.org/current/userguide/idea_plugin.html) for this). Again, this adds complexity and makes the software fragile (because newer IDEA versions may also have another XML format).

## Simpler Solution

There is no perfect solution for this, but if you do _not_ rely on [gradle's resource processing features](https://dzone.com/articles/resource-filtering-gradle)[^2] you can just throwing 'everything' together, e.g. via


````groovy
allprojects {
  apply plugin: 'idea'
  idea {
    module {
      inheritOutputDirs = false
      outputDir file("$buildDir/intellij")
      testOutputDir file("$buildDir/intellij")
    }
  }
}
````

This is easy to set up (should be picked up by IDEA automatically, otherwise just run `gradle idea`), easy to debug (if a resource or bean in the same module cannot be found, just check the contents of`build/intellij`), and easy to use (it is separate from your gradle build output).

----

[^1]: Unfortunately, this is not configurable either.
[^2]: Actually a nice feature, and still useful in all other kinds of files that are not crucial for your Spring Boot setup.
