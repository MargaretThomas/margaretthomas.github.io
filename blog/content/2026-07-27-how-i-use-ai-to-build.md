---
title: "How I Use AI to Build"
summary: "A practical look at how I use ChatGPT to think, Codex to implement, and how I review the changes"
date: 2026-07-27
tags:
  - AI-Assisted Development
  - Behind the Build
slug: how-i-use-ai-to-build
---

# How I Use AI to Build

I build software with the use of AI.

My process goes like this:

**I think with ChatGPT. I build with Codex. I review everything in between.**

## ChatGPT: the thinking happens here

ChatGPT is usually where a project starts.

I use it to:
- untangle ideas
- define features
- question decisions
- plan technical approaches
- turn vague thoughts into scoped prompts

Eventually, my conversations with ChatGPT turn into README files, project status files, project roadmaps and feature prompts.

## Codex: this is where the prompts turn into code

Once I know what I want, I give Codex a focused prompt.

Codex works inside the repository, reads the existing code, makes changes, and
helps implement the next slice of the project.

I use Visual Studio Code with the Codex extension. Usually I'll make a new chat for each feature or query. Most of the context is already in the repo (existing code & docs) and in the prompts.

My loop is roughly:

**Idea → ChatGPT → Prompt → Codex → Review → Test → Commit (code, then prompt)**

I keep going, until the features are implemented, bugs are addressed and the code is tested & improved.

## This portfolio is an example

When I revamped this portfolio, I gave Codex a prompt describing:

- the visual direction
- the site structure & content
- technical constraints

The site had to stay simple: plain HTML, CSS and JavaScript, with no framework
or build step.

You can see the original prompt here:

[View the portfolio build prompt on GitHub](https://github.com/MargaretThomas/margaretthomas.github.io/blob/main/prompts/001-build-the-portfolio.md)

I like keeping important prompts in the repository because they become part of
the project history.

A code commit shows what changed.
A prompt commit shows what I was trying to make happen.
I separate the code commits from the prompt commits. Prompts are committed after the code changes are.

## The first of many

I'll be using my portfolio as a home for my blogs.
This is the first blog of many.

Going forward, expect to see blogs about:
- How I am building Thin Walls: From an idea to a working system
- How I am building the world around Thin Walls
- A technical dive into how I am building the Thin Walls mobile app
- A technical look at how I am building the backend for the Thin Walls mobile app

That's it for this blog. See you next time.

Happy building,  
Margaret
