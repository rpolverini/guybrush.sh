---
layout: post
title: "My AI stack for weekend coding without burning the company's API"
date: 2026-09-05
categories: [ai, development, cli, tools]
tags: [opencode, gemini, siliconflow, openrouter, ai-coding, cli]
lang: en
ref: ai-weekend-stack
---

There is probably a situation you know.

It's Friday night.

You have an idea.

It's a beautiful idea.

It probably will not make you any money.

It is probably not especially necessary either.

But **you have to build it**.

The problem appears when you open your favorite coding agent and remember that the API configured in it belongs to the company.

And then that little voice appears:

> "Hey... are we paying for this?"

No.

**This project is mine.**

Besides... it's already Saturday.

So the company API stays closed :D ... well, no, using it for this would be wrong... which means it is time to find a tool for playing with AI without mortgaging the paycheck.

# The problem: I want Claude Code, but for my silly projects

For quite a while I had a solution I liked a lot:

**trybons.ai.**

The idea was fantastic: a CLI acting as an intermediary and letting you try different AI models for programming.

It was exactly the kind of tool you want for a personal project:

```text
terminal
   │
   ▼
   Bonsai
   │
   ├── model A
   ├── model B
   ├── model C
   └── model D
```

The problem with these things is that there is an important difference between:

**"this works for free"**

and

**"this works for free while someone else keeps paying for the party."**

And apparently Bonsai's party is over... or someone forgot to water the little tree.

¯\*(ツ)\*/¯

So it was time to find a replacement.

# The first thing I learned: the trick is not finding "free AI"

My first instinct was to search for:

> "What is the best free API for programming?"

But I think that is the wrong question.

The interesting question is:

> **What is the best CLI that lets me switch providers without changing the way I work?**

That is where **OpenCode** comes in.

OpenCode is a coding-agent tool for the terminal that does not try to force you into using a single provider.

It currently supports a huge number of providers and also allows you to use local models.

The architecture starts looking pretty nice:

```text
                    ┌── OpenRouter
                    │
                    ├── SiliconFlow
                    │
OpenCode ───────────┼── Groq
                    │
                    ├── Gemini
                    │
                    ├── OpenAI
                    │
                    ├── Anthropic
                    │
                    └── Ollama
```

And that completely changes the problem.

I am no longer looking for **"my new AI."**

I am looking for **a good router for my terminal.**

# OpenCode: the missing piece

The beauty of OpenCode is that the tool and the model stop being the same thing.

I can have:

```text
opencode
```

as my working interface and then decide where to send the queries.

For example:

```text
OpenCode
   │
   └── OpenRouter
          │
          ├── free model
          ├── another free model
          └── paid model
```

Or:

```text
OpenCode
   │
   └── SiliconFlow
          │
          ├── Qwen
          ├── GLM
          ├── Kimi
          └── other models
```

Or even:

```text
OpenCode
   │
   └── Ollama
          │
          └── local model
```

And that last option has a rather interesting property:

**marginal API cost = $0.**

Because you are the server.

Well...

You still have to pay for electricity.

But if AI starts charging you per watt, we have a different problem.

# What about SiliconFlow?

Here comes a small clarification regarding several lists circulating online.

SiliconFlow is often recommended as a free provider of Chinese models, especially DeepSeek, Qwen, GLM, and so on.

But **I would not assume that SiliconFlow is currently an unlimited free API**.

Its current catalog is huge, with more than 200 models, and it offers an API compatible with common providers, but its current commercial model is mainly pay-as-you-go.

For example, it currently lists models such as:

- GLM
- Kimi
- Gemma
- GPT-OSS
- various reasoning models
- models specialized in coding

And some of them are genuinely interesting for programming.

Kimi K3 is even available on SiliconFlow, with a context window of around 1 million tokens.

But if my goal is:

> **"I do not want to spend a cent this Saturday"**

then I am not going to build my entire strategy around a provider that charges per token.

I will treat it as **an interchangeable component**.

That difference matters.

# So... what would I use for free?

Here is another pretty interesting option:

## Gemini CLI

Google has its own open-source CLI for using Gemini from the terminal.

And its current free tier is quite generous:

```text
60 requests / minute
1000 requests / day
```

with a personal Google account.

That is a lot for a personal weekend project.

Especially because you often do not need:

```text
THE ULTIMATE FRONTIER MODEL
```

to say:

```text
"Hey, build me an endpoint for this"
```

Sometimes a reasonably good model is perfectly adequate.

# My Saturday stack

So if I had to put together my setup for personal projects today, I would think about it like this:

```text
                  MY PROJECT
                       │
                       ▼
                    OpenCode
                       │
          ┌────────────┼────────────┐
          │            │            │
          ▼            ▼            ▼
      Gemini CLI   OpenRouter   SiliconFlow
          │            │            │
          ▼            ▼            ▼
       Gemini       free         open
                    models       models
```

And if the project grows:

```text
                  OpenCode
                     │
       ┌─────────────┼─────────────┐
       │             │             │
     Free          Cheap         Local
       │             │             │
    Gemini       SiliconFlow    Ollama
    OpenRouter    OpenRouter
```

The tool remains.

The backend changes.

**That is the real superpower.**

# What about OpenRouter?

OpenRouter is another very interesting piece for this setup because it acts as a unified layer in front of many models.

OpenCode supports it directly and lets you select models from within the interface.

It is exactly the concept I was looking for after Bonsai:

```text
OpenCode
    │
    ▼
OpenRouter
    │
    ├── free model
    ├── cheap model
    ├── experimental model
    └── "I want to try this one today"
```

That lets me switch models without turning my `~/.bashrc` into an archaeological graveyard of API keys.

# What about free Claude?

We also need to be careful with the lists we find floating around online.

For example, some recommendations still talk about DuckDuckGo as if it offered:

```text
Claude 3.5 Haiku
Llama 3.3
GPT-4o mini
o3-mini
```

But that is already outdated.

Duck.ai's current free offering includes, among others:

```text
Claude 4.5 Haiku
Mistral Small 4
GPT-5.4 nano
GPT-5.4 mini
gpt-oss-120b
Gemma 4 31B
```

according to DuckDuckGo's own documentation.

The lesson:

**do not copy a model list from a six-month-old article and assume it is still current.**

In AI, that is roughly equivalent to using Django 1.8 documentation to configure Django 5.

# The strategy I like

After looking at all these options, I do not think the answer is:

> "I found THE free provider."

That provider will probably change its terms within three months.

The more resilient strategy is:

```text
             ┌─────────────────┐
             │     OpenCode     │
             └────────┬────────┘
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
       Gemini     OpenRouter   SiliconFlow
          │           │           │
          └───────────┼───────────┘
                      │
                      ▼
                    Ollama
                  (if I feel like it)
```

In other words:

**the CLI is stable; the models are disposable.**

That seems much healthier to me.

# But there is one thing we must not forget

Free does not mean private.

If you are working with:

```text
API keys
credentials
customer data
proprietary code
secrets
environment variables
```

you should not assume that a free API is equivalent to running a local model.

For my personal Saturday project:

```text
personal project
public code
experiments
ideas
tests
```

perfect.

For the company repository:

```text
NO.
```

And especially:

```bash
export OPENAI_API_KEY=...
```

is not an invitation to paste the entire `.env` file into a prompt.

# So what would I install?

To get started quickly:

```bash
npm install -g opencode-ai
```

Then:

```bash
opencode
```

From there, I would configure whichever provider I wanted to use.

OpenCode lets you connect to providers with `/connect` and select models with `/models`.

Conceptually:

```text
/connect
```

choose:

```text
OpenRouter
```

enter the API key and then:

```text
/models
```

Choose the model.

The beauty is that tomorrow I can run:

```text
/connect
```

and switch providers.

I do not have to change tools.

# My ideal weekend setup

If I had to reduce it to a recipe:

### 🥇 To start for free

**Gemini CLI**

Because it has a generous free tier and is designed to work directly from the terminal.

### 🥈 To experiment with many models

**OpenCode + OpenRouter**

Because it decouples the tool from the provider and lets you jump between models.

### 🥉 To access many Asian and open-source models

**OpenCode + SiliconFlow**

Especially interesting if you want to experiment with Kimi, GLM, Gemma, GPT-OSS, and friends.

### 🏠 For total independence

**OpenCode + Ollama**

Because then you stop depending on an external API altogether.

# Now comes the fun part

There is something I especially like about this approach.

Suppose I am working on:

```text
santaclara.ar
```

and I want to run:

```bash
$ opencode
```

and start coding.

I do not particularly care whether the model behind it is:

```text
Gemini
Qwen
Kimi
GLM
DeepSeek
GPT
Claude
```

My workflow remains:

```text
terminal
   ↓
agent
   ↓
repository
   ↓
git
   ↓
commit
```

The model has become **interchangeable infrastructure**.

And to me, that is much more interesting than finding "the best model."

# Guybrush's conclusion

I think my initial mistake was looking for a replacement for Bonsai.

I do not need another Bonsai.

I need something better:

**a tool that makes Bonsai replaceable.**

That is where OpenCode makes a lot of sense.

Not because it is magically the best agent on the planet.

But because it lets me say:

> "Today I feel like using this model."

And tomorrow:

> "This model ran out of quota. Let's try another one."

And the day after:

> "Whatever. Let's install Ollama."

Without changing my entire workflow.

```text
                ┌─────────────┐
                │   OpenCode  │
                └──────┬──────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
     Gemini        OpenRouter    SiliconFlow
        │              │              │
        └──────────────┼──────────────┘
                       │
                    Ollama
                       │
                       ▼
                 "It's my machine."
```

And that is how I end up spending a Saturday programming a project I probably did not need to build...

**without spending the company's API budget.**

Because one thing is building unnecessary software.

Something entirely different is doing it **with someone else's credit card**.

---

_Guybrush Threepwood, programmer, musician, and occasional culprit behind starting yet another project on a Saturday at 11 in the morning._

> "If it works, do not touch it.
>
> If it is free, do not ask too many questions either.
>
> And if it is Saturday... build the project."
