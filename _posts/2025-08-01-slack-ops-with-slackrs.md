---
title:  "Weekend project: slackrs, a tool to analyze Slack data exports"
date:   2025-08-01
tags: chatops slack rust programming learning
---

As [already mentioned](/2025/04/21/gemini-as-programming-trainer.html) I've been starting to use an 'AI tutor' to learn a new programming language.[^AITutor] 
I've picked [Rust](https://www.rust-lang.org) for this because it has some interesting concepts that I want to understand at a deeper level (like its [ownership system](https://doc.rust-lang.org/1.8.0/book/ownership.html)), and the language is not exactly known for being [easy to learn](https://camo.githubusercontent.com/c54beef6967e7e099fab5ac6af561baeec99604d75505e05f84cfa77509a48c3/68747470733a2f2f70617065722d6174746163686d656e74732e64726f70626f782e636f6d2f735f353445314239364546464546443239343536323930324443354239393731443335434436423635304243383744313230303341333041343635313737363230315f313538363531343237353631385f696d6167652e706e67)... :-)

I also sometimes encounter Rust code in my day job, so knowing a bit more about it will have some additional utility and fills a niche in my 'personal toolchain' (where most tools are memory-managed). On top of that, Rust has some features I care about in any language, such as an expressive type system and nice developer tooling.

## slackrs

So, lo and behold, I have now sort-of 'vibe-coded'[^VibeCoding] [`slackrs`, a small weekend project for "SlackOps"](https://github.com/roland-ewald/slackrs/): it ingests the message data exports from public channels that you can export from [Slack](https://slack.com/) for a certain time range, and then generates plots on the frequency of specific terms (see the [README](https://github.com/roland-ewald/slackrs/blob/main/README.md) for more details).

This is useful for analyzing trends on specific auto-generated slack messages (e.g. for deployments, builds, or test runs and their failure rates), as well as messages between certain group or user handles, e.g. to see how the communication workload of a team fluctuates over time.

The tool is still very basic, just covering my own requirements, and will probably remain so. Still, it was a very fun exercise and you can now just try out [the crate](https://crates.io/crates/slackrs) or [the source](https://github.com/roland-ewald/slackrs).

--

[^AITutor]: AI Tutoring really flattens the learning curve, which also means I can now dive into other topics more quickly. This is a great and sometimes undervalued benefit of LLMs. Validating any claims (e.g. by checking the documentation) and _thinking through_ code snippets is naturally part of the learning process anyhow, as well as trying them out in practice --- so hallucinations do not really matter in this use case. They even make using LLMs _more_ fun: it means your 'tutor' is known to be a bit unreliable and you should (just as in real life) still use your _own_ head and challenge any statements you find implausible.

[^VibeCoding]: It was more like "assisted coding" as I was mostly _learning_ (new syntax etc.), so the actual programming was still mostly done by me. Obviously, all _remaining_ bugs in the project are the LLM's fault, not mine! ;-)