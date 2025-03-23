---
title:  "FutureHouse's AI research toolkit"
date:   2025-03-23
tags: ai computer-science genetics biology automation literature-search
---

[Asimov Press](https://www.asimov.press) recently published [an interesting interview](https://www.asimov.press/p/futurehouse) with [FutureHouse](https://www.futurehouse.org), a San Francisco non-profit that tries to automate scientific discovery. They are developing several practical AI-powered tools to better work with research literature[^1], and three of them seem particularly interesting to me.

### PaperQA: research summarizer and 'semantic linter'

In their [paper on PaperQA](https://arxiv.org/abs/2409.13740), a 'language model agent', they show it can not only summarize papers (better than Wikipedia; more on that below) but can also find _contradictions_ in papers.
They tested this with randomly sampled biology papers and early results are promising (n is only 93, though):

> PaperQA2 identifies 2.34 +/- 1.99 contradictions per paper in a random subset of biology papers, of which 70% are validated by human experts. These results demonstrate that language model agents are now capable of exceeding domain experts across meaningful tasks on scientific literature.

I could see many practical uses of such a tool _besides_ classic literature search, because I'm sure most of the contradictions the tool found are benign and a result of sloppy writing or otherwise avoidable errors. This would be quite useful when preparing a manuscript, in the same way that programmers rely on [linters](https://en.wikipedia.org/wiki/Lint_(software)) to identify problematic code in a codebase.[^2]

As PaperQA is [open source](https://github.com/Future-House/paper-qa), you can play around with it yourself.[^3]

### Has anyone ...?

I'm also curious about how well [hasanyone.com](https://hasanyone.com) will work[^4], as this might be a much nicer interface for literature search than Google scholar or using 'vanilla ChatGPT'. The idea is straightforward: just ask in plain text if there is scholarly work on a specific problem, crucially one that you can describe _in your own words_[^5]. If I understand this correctly, the service then uses an LLM to map your question onto similar concepts across the various scientific subdomains that may have dealt with it, and the result is a brief summary and a list of references.
This could be a boon for finding related work and could help to avoid re-inventing the wheel.[^6]


### WikiCrow as LLM-generated bottom-up OMIM?

Another cool tool that apparently came about by applying [PaperQA](https://arxiv.org/abs/2409.13740) to Wikipedia is [WikiCrow](https://wikicrow.ai), which provides a Wikipedia-style page for all[^7] human genes, including a nice description and further literature.

I'm not a clinical geneticist but I _know_ that resources like Genomenon's [Mastermind database](https://mastermind.genomenon.com/) or [OMIM](https://www.omim.org/) (**O**nline **M**endelian **I**nheritance in **M**an)[^8] are crucial for diagnostics, and having more options here is always a good thing.
When comparing the results of WikiCrow for a well-known gene like [`BRCA2`](https://en.wikipedia.org/wiki/BRCA2), you can see that [its results](https://wikicrow.ai/BRCA2) are 'kind of OK', but other sources still have more and better information:

- [Mastermind](https://mastermind.genomenon.com/gene?gene=brca2&gene_op=and) shows almost 60.000 papers and provides variants as well as relations to diseases and gene-specific metrics that are relevant for a diagnosis (such as gnomAD's [LOEUF](https://gnomad.broadinstitute.org/news/2024-03-gnomad-v4-0-gene-constraint/#loeuf-guidance)).

- [OMIM](https://www.omim.org/entry/600185) provides much more details on the different aspects to consider (gene structure, phenotype correlations, and so on), as well as ca. 100 _curated_ paper references.

- [WikiCrow](https://wikicrow.ai/BRCA2) shows 15 references (this might be limited by the LLMs output size limitations), fails to show `BRCA1` as a 'related gene', and has some other peculiarities: genomic coordinates are given only for hg19 (the current reference genome is hg38, released end of 2013), the gene's strand is given as '1' (customary notation would be '+' and '-', or 'forward' and 'reverse'), and genomic coordinates as well as the number of exons are _transcript-specific_, and `BRCA2` has several transcripts[^9] _with differing exon count_ (so it does not make sense to state that `BRCA2` has 26 exons).

In summary: this is a really cool idea, but it's not quite there yet. I wonder how far the FutureHouse team can push this, though, because my nitpicks are totally solvable problems and I would really like to see how good a service that _augments_ the basic bioinformatics data (for which you _don't_ need an LLM) with LLM capabilities can become.


--

[^1]: They provide Apache-licensed open-source tools [on their GitHub account](https://github.com/Future-House).

[^2]: The more 'AI-centric' view on this would be that PaperQA could be a great critic for the LLM that _generates_ the manuscript in the first place, i.e. it could be one part of an [actor-critic approach](https://en.wikipedia.org/wiki/Actor-critic_algorithm).

[^3]: Word of warning: the [installation](https://github.com/Future-House/paper-qa?tab=readme-ov-file#installation) requires not only an OpenAI API key (using other models is possible) but also recommends [Crossref](https://www.crossref.org) and [Semantic Scholar](https://www.semanticscholar.org) API keys for more intensive use, because you will probably hit the rate limits quickly. If you don't have either, the Crossref membership is [at least $275 per year](https://www.crossref.org/fees/#annual-membership-fees), and [Semantic Scholar does not give out any API keys at the moment](https://www.semanticscholar.org/product/api#api-key-form) (you have to join a waitlist).

[^4]: Unfortunately, they have stopped sign-ups for now, and judging by the API and cost details that briefly appear when you try one of the pre-defined 'search suggestions', it's just quite costly to run this at scale, for now (I wish they would just let me enter an OpenAI API key).

[^5]: I've found that one's (lack of) knowledge regarding the jargon of a field is often a limiting factor when using search engines such as Google scholar for literature search.

[^6]: Although there is also something to be said _for_ occassionally reinventing the wheel, as [this nice article by Tobias Løfgren](https://tobloef.com/blog/wheel-reinventors-principles/) ([via HN](https://news.ycombinator.com/item?id=43434730)) points out.

[^7]: _All_ human genes? Well, there are still known protein-coding genes like [`C6orf58`](https://www.genenames.org/data/gene-symbol-report/#!/hgnc_id/HGNC:20960) that even have [a MANE Select transcript (`NM_001010905.3`)](https://www.ncbi.nlm.nih.gov/nuccore/NM_001010905.3) and while [WikiCrow _knows_ them](https://wikicrow.ai/C6orf58), it "[...] failed because insufficient scholarly sources were found". Genetics is far from being solved.

[^8]: If you want to learn more about the history of OMIM---which started _offline_ in 1966 and was consequently just called 'MIM' back then---and its founder, [Victor McKusick](https://en.wikipedia.org/wiki/Victor_A._McKusick), I recommend the wonderful book by Rebecca Skloot, [_The Immortal Life of Henrietta Lacks_](https://www.amazon.com/dp/1400052181).

[^9]: You can see this for yourself in the [NCBI's Genomic Data Viewer](https://www.ncbi.nlm.nih.gov/gdv/browser/gene/?id=675).