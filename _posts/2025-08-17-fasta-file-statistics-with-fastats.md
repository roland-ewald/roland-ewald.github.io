---
title:  "Weekend project: fastats, a small bioinformatics tool for FASTA file archeology"
date:   2025-08-17
tags: rust programming bioinformatics
---

As another little side-project [for learning Rust](/2025/08/01/slack-ops-with-slackrs.html) I wrote a small command-line utility, `fastats`, to help with "_reference genome archeology_". With this, I unfortunately don't mean some cool paleogenetic research, but instead the more annoying problem of searching for subtle differences in reference genome files (usually stored as [`*.fasta`](https://en.wikipedia.org/wiki/FASTA_format) files), e.g. when troubleshooting alignment problems or changes in downstream results.

Preparing a genomic sequence as a [reference genome](https://en.wikipedia.org/wiki/Reference_genome#Human_reference_genome) works about as well as [any standardization effort in technology](https://xkcd.com/927/), which means there are _many_ reference genome files available by now, even for the same 'build' (like hg19 or hg38). The problem is bad enough that there are [long support articles](https://emea.illumina.com/science/genomics-research/articles/dragen-demystifying-reference-genomes.html) to help users understanding the differences between the files.

This problem is addressed by `fastats`, as it tries to deliver a better _description_ of a given FASTA file by generating overall statistics per sequence (such as the GC content, or a hash of the sequence) and defining which genomic regions are soft- or hard-masked. The masked (and non-masked) regions are stored in [BED files](https://en.wikipedia.org/wiki/BED_(file_format)), so that it is very easy to look them up.[^Masking]

This should make it much easier so spot relevant differences between FASTA files. The tool is available on [crates.io](https://crates.io/crates/fastats) and [GitHub](https://github.com/roland-ewald/fastats).

--

[^Masking]: Masking is an important technique in reference genome curation, as it allows to 'rescue' certain regions that might otherwise not be visible (e.g., because they are duplicates, and thus aligners may non find a non-ambiguous alignment). Soft-masking means changing the capitalization of the sequence (i.e., writing `acgt` instead of `ACGT`), so the sequence is still available. Hard-masking, on the other hand, replaces a specific region of the sequence with `N`, i.e. there could be _any_ nucleotide at this position (which means the aligner will not align any reads in that region).
