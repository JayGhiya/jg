---
date: '2025-11-04T10:33:43+05:30'
draft: false
title: 'Why Unoplat Code Confluence'
tags: ["open-source", "developer-tools", "code-intelligence", "startup-journey"]
categories: ["Startup Journey"]
description: "The story behind building Unoplat Code Confluence - an open source code intelligence platform after seven years in corporate engineering"
---

**TL;DR**
We are building an open source code intelligence layer after seven years in corporate engineering. The core insight to start the venture was agents/humans waste ton of time rediscovering code and its metadata based on any task at hand. We want to build a precise structured information that keeps context tight and task specific.

## The Itch

Seven years in corporate engineering gave me discipline and scale. I do my best work when I can own the stack, ship in tight loops, and see impact quickly. To align how I work with what I value, I set out to build open-source developer tooling.

## The Spark: 2024

In end of 2024, my former tech lead [Vipin Kumar](https://www.linkedin.com/in/vipinshreyaskumar/) proposed a project: extract insights from codebases by leveraging comments placed above methods and classes—comments written by both humans and code assistants. The goal was simple: ease onboarding for engineers new to a codebase. The problem isn’t new; anyone switching companies, teams, or even services within the same org has felt this friction.

### Prototypes and Reality Check

I began with what I knew as a java backend engineer: Java AST for analyzing Java repositories. Simultaneously, I experimented with state-of-the-art assistants like Cursor and Codeium. The more I explored, the clearer it became that comment-only intelligence wouldn't sustain an impactful product. I realized the entire approach needed to be rebuilt from the foundation.

## The Realization

To build precise, efficient context for large codebases, you need strong code grammar foundations. I upskilled by studying existing projects that leveraged code grammars effectively.
Deterministic and reliable impact across software engineering tasks requires formal grammars/static code analysis. If solved in the right manner more pain points like dependency graphs , architecture checks etc can be tackled precisely as the approach is foundational and scalable.

### The Decision

I resigned to start an open-source developer tool: **Unoplat Code Confluence**.
The plan: first build precise and efficient context for codebases, then enable AI-first use cases on top of that foundation.

## What is Unoplat Code Confluence?

[Unoplat Code Confluence](https://github.com/unoplat/unoplat-code-confluence) is a codebase intelligence platform that:
- Uses formal grammars and schemas rather than brittle heuristics
- Creates reliable, structured context from your code
- Enables reliable , auditable and efficient AI workflows only after the foundational context is trustworthy

The product is built on these core values:
- **Precision**: Grammar-based analysis, not regex or llm guesswork
- **Efficiency**: Process large codebases without drowning in noise
- **Reliability**: Consistent results you can trust
- **Self-hosting**: Your code stays on your infrastructure
- **Transparency**:  Open Source

The first AI usecase it aims to solve is:
- Generate a comprehensive **AGENTS.md** per repository that captures sections such as **Development Workflow** , **Dependencies Guide** , **Business Logic** etc through deterministic code grammar and agents, with incremental auto-updating and more sections on the [roadmap](https://docs.unoplat.io/docs/introduction/roadmap). Here is the alpha demo on [Youtube](https://www.youtube.com/watch?v=fRBV_f9fDKc&list=LL).

## Why It Matters

Onboarding, cross-repo understanding, bugs, launching new features and use cases involving discovery ,code collaboration, maintenance, and enhancement remain painful when agents have to do trial and error based pattern search based on their knowledge and this is worst for non-sota open source models who are good at edits but are not so good at creating a plan of where to edit based on feature/business domain/framework understanding.

We generate a comprehensive Agents.md that gives agents precise context so they understand your business domain better, find the right spots thereby making better changes leading to overall better developer experience. Incremental auto-updating to keep Agents.md in sync as code evolves is [on the roadmap](https://docs.unoplat.io/docs/introduction/roadmap).

So also as part of [context engineering principles](https://www.llamaindex.ai/blog/context-engineering-what-it-is-and-techniques-to-consider?utm_source=chatgpt.com) it is proven that agents work best when they see only the right code at the right time. Long noisy context hurts retrieval and accuracy.

### What's Next
I'm planning a series of posts:
- Lessons learned from approximately 9 months of full time building.
- Current Progress
- The road ahead: Roadmap

If you're interested in better code intelligence tooling or want to follow this journey, reach out on [X](https://x.com/BeLikeBumblebee) or [LinkedIn](https://www.linkedin.com/in/jay-ghiya/), or join our [Discord community](https://discord.gg/xcV9PxpS4J).
