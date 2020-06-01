---
title:  "IntelliJ IDEA plugin for bulk-renaming Java types"
date:   2020-06-01
tags: intellij idea plugin refactoring
---

Last week I wanted to rename a larger amount of Java types so that they are consistent with a new naming scheme. 
This does not seem to be easily possible in IntelliJ, so I wrote a [small plugin](https://github.com/roland-ewald/intellij-bulk-rename/) that imports a CSV file with 
the 'refactoring instructions' and runs them in bulk. 
It also allows to define type-specific refactoring options, such as telling IntelliJ to also replace occurrences of the type name in other text files. 

The developers at JetBrains really put some effort into supporting plugin developers. There are many helpful context-specific messages, 
e.g. to inform you about APIs that are soon to be deprecated, or what exactly you forgot to put into your `plugin.xml`. 
The latter even comes with a custom auto-complete for module dependencies. 
There were a few minor roadblocks[^1], but nothing serious. This is the power of dogfooding, I guess. 

### Downloader beware

This was a _weekend project_, so while I'm happy with the refactoring result itself, there are many rough edges (and I'm pretty sure there are better ways to use the IntelliJ API). 
Nevertheless, it may be a useful blueprint for implementing similar utilities that help with larger 'one-off' refactorings.


----

[^1]: For example, the predefined gradle setup requires more than the default RAM on my machine, so a [custom setting in the `gradle.properties` file](https://github.com/roland-ewald/intellij-bulk-rename/blob/fafea9d8c7dc9cfc190db517fdcb9e04ed03d506/gradle.properties#L4) was required to initially configure the project.