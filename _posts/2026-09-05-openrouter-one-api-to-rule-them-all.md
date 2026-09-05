---
layout: post
title: "OpenRouter: one API to rule them all"
date: 2026-09-05
tags: [openrouter, ai, api, llm, experimentation]
lang: en
ref: openrouter-api
---

There are weekends when you just want to program something peacefully.

Without touching production.
Without spending the company's tokens.
And, preferably, without having to pull out a credit card for every experiment that crosses our minds at 2 a.m.

That's where **OpenRouter** comes in.

## What is OpenRouter?

OpenRouter is, basically, an API that lets us access AI models from different providers in one place.

Instead of having:

- one API for OpenAI,
- another for Anthropic,
- another for Google,
- another for DeepSeek,
- another for whatever model appeared this week...

we have a single entry point.

And we can choose which model we want to use.

It's particularly useful for experimenting because switching models does not mean rewriting our entire application.

## What makes it special?

We can try very different models through a fairly similar interface.

For example, we might have a project using a Google model today and want to try DeepSeek, Qwen, or some other model tomorrow.

Instead of changing our whole integration, we change the model.

It's a small difference that becomes a pretty big one once we start experimenting with several LLMs.

## Is it free?

Here comes the part that interests those of us on a budget.

**Yes, there are models available for free.**

**But free does not mean unlimited AI until LeChuck rises for the fifth time.**

Free models have considerably tighter usage and availability limits than paid models.

For trying things out, running small experiments, playing with a CLI, or developing over a weekend, they may be more than enough.

When we need more volume, better limits, or want to use models without a free tier, paid credit comes into play.

The nice part is that we can add credit and use pay-per-use models without maintaining a separate account and integration for every provider.

## How do we get started?

First, we go to [OpenRouter](https://openrouter.ai/) and create an account.

Once we're in, we go to the **API Keys** section and generate a new key.

One important thing: treat that API key as what it is.

**A password.**

Do not upload it to GitHub, paste it into a README, or, even worse, put it in a `print()` and then forget to remove it.

The idea is to store it as an environment variable:

```bash
export OPENROUTER_API_KEY="..."
```

And that's it.

We now have a credential we can use from our tools.

## What now?

This is where things get interesting.

We can use the OpenRouter API from our own applications, for example from **Python**, but we can also integrate it with tools that support compatible providers.

For example, if we're playing with a coding agent such as **OpenCode**, we can configure access through OpenRouter and start trying different models.

That lets us do something pretty fun:

**try models without marrying one of them.**

And for an experimental project, that's gold.

Today we try a free model.

Tomorrow we find one that reasons better.

The day after, another one shows up that costs half as much.

We change the model and keep working.

## So what is it useful for?

To me, OpenRouter's main advantage is not simply "having lots of models."

It's having **a common gateway for experimenting with them**.

If you're learning about AI, building a personal project, or simply trying to find out which model works best with your code, it removes a lot of friction.

It also has one fundamental advantage on a Saturday afternoon:

**you don't need to turn your credit card into development infrastructure.**

Coffee is expensive enough already.

---

_Guybrush Threepwood, future pirate of the Caribbean and current user of artificial intelligence APIs._

> "Never pay for three APIs when you can argue with one."
