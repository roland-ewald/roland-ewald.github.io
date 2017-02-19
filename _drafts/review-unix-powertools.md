---
title:  "Review: Unix Power Tools"
tags: unix linux review book reading-tips
---

The [Humble Bundle on Unix](https://www.humblebundle.com/books/unix-book-bundle) was too good an opportunity to pass, but I was a little on the fence regarding the age of most books, and whether they have 'aged well' or not.

I started with 'Unix Power Tools', which seemed to be one of the 'classics' and a good starting point before digging deeper. The book really does contain a treasure trove of Unix specifics, most of which are still applicable today. Even the things that are _not_ relevant nowadays are an interesting read, as they let you reflect on the progress that has been made, and also in which way the new tools are better (or even just a little more standardized, which also helps a lot). One example of this would be the extensive discussion of `diff`, which I suspect is for most users replaced by the version control's `diff` utility by now.

### Reading vs. Skimming

- just for backup means I will not use it, reading front to cover is pointless: but skimming and checking whether the code samples are understandable is a great way to learn more about shell scripts and shell idiosyncrasies

- multiple solutions are often given for the same task, with different 'boundary conditions' (different shells, different approaches)

### Random things I picked up

Here are a few examples:

- Minor things like some command-line shortcuts I was not aware of (`Ctrl+U` and `Ctrl+W` to delete lines and words, respectively).

- Which tools are involved in the `man` pages system, and how to write one yourself (not that I plan doing that, the syntax is pretty awkward -- but still, good to know 'how the sausage gets made').

- How terminal emulators work and what problems they solve, on a more technical level.

- How to update the file database for `locate`, and how to build your own databases to locate specific files.

- What's inside `/etc/magic` (BTW I think it's hard to come up with a worse file name for such a mundane task. This will be my new example of 'Naming Things is Hard'.) (Spoiler: how to detect file types. Magic!)

- Lots and lots of useful scripts (for different shells), which highlights the similarities and differences between shells, and gives you a better feeling of how to combine `find`, `grep`, `sed`, `awk`, `xargs`, and so on in your own scripts. Typical corner cases and problems (escape characters, different meanings of wildcards, limitations) are also described.

## Summary

I am pretty sure that most readers would pick up different things than I did, but I am pretty sure almost anyone below the linux expert level will at least find some interesting recipes for further customization, or at least some interesting historical perspective. In any case, this was a good read. Next on my list:
