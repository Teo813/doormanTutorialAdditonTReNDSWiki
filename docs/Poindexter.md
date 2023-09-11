---
layout: default
title: Modules
nav_order: 2
parent: List of software
last_modified_date: 09/11/2023 10:32
---
<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# How to Use Poindexter
## What is Poindexter?
**Poindexter** is a Slack bot designed to summarize and simplify research papers from arxiv.org links. He is primarily designed for usage by researchers and those looking to entice other Slack users to read their papers.
## How do I use Poindexter?
Poindexter can be triggered in two main ways. Both ways do share a feature however. Poindexter looks for a given keyword to help him understand the age level of a given summary. If no keyword is given he defaults to a child level. Otherwise the keywords are "child", "teenager", "undegraduate", "graduate", and "phd".
### Auto Replies
Poindexter's primary method of usage is by sending an arxiv.org link in any chat he is added to. He will automatically seek out and reply to these. If an age keyword is given he will automatically tailor his output for a given group. He will reply within a thread.
### Slash Commands
Poindexter can also be used by calling the command "/dumbitdown [LINK] [AGE KEYWORD]". This will send Poindexter's reply as a message outside of a thread.
