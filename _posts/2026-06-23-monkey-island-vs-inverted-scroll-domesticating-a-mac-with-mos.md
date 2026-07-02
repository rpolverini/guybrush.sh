---
layout: post
title: "Monkey Island vs Inverted Scroll: domesticating a Mac with Mos"
date: 2026-06-23
categories: [macos, productivity, tools]
tags: [mac, mos, smooth-scrolling, mouse, productivity, career-change]
lang: en
ref: monkey-island-mos-scroll
---

# Monkey Island vs Inverted Scroll: domesticating a Mac with Mos

There are moments in life when you have to make tough decisions.

Leaving a startup.
Changing jobs.
Quitting Windows.

Well, let's not exaggerate.

The thing is that in this new job mission an unnegotiable condition appeared:

> "We give you a Mac."
And there I was.

A brushed aluminum spaceship.
A processor that seems fed by plutonium.
A battery that lasts longer than some Latin American governments.

But with one small problem.

**The scroll was possessed.**

---

## The first encounter with the curse

Monkey Island veterans know a curse when they see one.

I opened Chrome.
I moved the mouse wheel down.

The page scrolled up.

I moved it again.

The page kept going up.

I thought I was tired.

I tried again.

No.

The Mac was convinced my brain worked backward.

---

## Apple's official explanation

Apple calls this:

> Natural Scrolling
Which is a fancy way of saying:

> "Let's assume everyone uses a trackpad."
And there's the problem.

When you scroll with your fingers on a trackpad, the movement has a certain logic:

- You push the content up.
- The content moves up.
- You see lower content.
Perfect.

But when you connect a mouse like a civilized person who spent decades using computers...

...that logic becomes a kind of voodoo ritual.

You move the wheel down.

The screen goes up.

Your brain enters an existential conflict.

---

## The real problem

The madness doesn't end there.

macOS has a single setting for both devices:

- Trackpad
- Mouse
If you disable inverted scroll:

- The mouse behaves.
- The trackpad feels weird.
If you enable it:

- The trackpad behaves.
- The mouse feels weird.
It's as if in Monkey Island they decided the same key opens every door in the Caribbean.

---

## The solution: Mos

After roaming taverns, secret caves, and GitHub repositories, the magical object appeared.

### Mos

Mos is a free utility for macOS that does two extremely important things:

1. It lets you configure mouse scroll separately.
2. It adds real smooth scrolling.
In other words:

- Trackpad happy.
- Mouse happy.
- User happy.
Something Apple apparently left optional.

---

## Installation

We went to the official site:

### Official site
👉 [https://mos.caldis.me](https://mos.caldis.me/)

Or directly to the repo:

👉 [https://github.com/Caldis/Mos](https://github.com/Caldis/Mos)

We downloaded the latest version and installed it normally.

The first time macOS will probably try to protect you from yourself.

A dramatic message like this will appear:

> "This app was downloaded from the Internet."
We accept.

We breathe.

We keep going.

---

## Required permissions

Once Mos is open, it will likely ask for accessibility permissions.

Go to:

```
System Settings
→ Privacy & Security
→ Accessibility
```

And enable:

```
Mos
```

Without that it won't be able to intercept mouse events correctly.

---

## Recommended configuration

Once installed, an icon appears in the top bar.

Open Preferences.

Like in the screenshot:

[![Mos Preferences](https://github.com/assets/img/posts/mos-preferences.png)](https://github.com/assets/img/posts/mos-preferences.png)

### General
Enable:

```
☑ Launch on Login
```

So we never have to remember it again.

---

### Scrolling
This is where the real magic happens.

Configure:

```
☑ Smooth Scrolling
☑ Reverse Scrolling (only for mouse if you prefer)
```

The idea is to recover decades of muscle memory without ruining the trackpad experience.

---

### Adjusting the feel

Every mouse is different.

I recommend starting with:

```
Step: Medium
Duration: Medium
Acceleration: Low
```

And then tweak it to taste.

The goal is not to turn scrolling into a roller coaster.

The goal is to make it stop feeling like a wheelbarrow on cobblestones.

---

## The difference after installing it

Before:

- Weird scroll.
- Jerky jumps.
- Strange feeling in Chrome.
- Strange feeling in VSCode.
- Strange feeling everywhere.
After:

- Consistent scroll.
- Smooth movement.
- Mouse working like anyone would expect.
- Brain stops insulting Apple every five minutes.

---

## Is it worth it?

Absolutely.

If you come from Linux or Windows and use a physical mouse every day, Mos is probably one of the first apps you should install.

The Mac will still have some peculiar habits.

But at least it will stop arguing with your muscle memory every time you try to scroll down a page.

And in the digital Caribbean, as we learned years ago, avoiding unnecessary fights always leaves more time to hunt for treasure.

---

## Survival kit for a pirate new to macOS

My initial list after landing on the island:

- Mos → fix scroll.
- Rectangle → manage windows like a normal person.
- Raycast → replace Spotlight.
- iTerm2 → proper terminal.
- Stats → lightweight system monitoring.
But that is a story for another voyage.

---

> "Never trust an operating system that tells you moving a wheel down means going up."
> 
> — Guybrush Threepwood, probably.
