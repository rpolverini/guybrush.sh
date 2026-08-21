---
layout: post
title: "LibreWind - Stage 0: The laboratory and the birth of LibreWind S"
description: "LibreWind begins: a free, affordable and resilient wind instrument designed to help children get closer to music. In this stage we measure materials, choose the mouthpiece and build the first experimental prototype."
date: 2026-08-20
categories: [librewind, music, lutherie]
tags: [librewind, librewind, music, lutherie, diy, open-source, instruments, ppr, saxophone, wind]
lang: en
ref: librewind-s-stage-0
---

# LibreWind

## Codename: Caña Libre

> **Music deserves to be free too.**
>
> Because a man cannot live on engineering alone.

Sometimes you have to step away from the keyboard for a while, pick up a caliper, a piece of PP-R fusion pipe and a saxophone mouthpiece, and ask what would happen if we tried to build a musical instrument that a child could use without the price of the instrument being the first barrier.

And if, along the way, we manage to make that instrument survive a six-year-old...

Well.

That would be practically military technology.

# What are we building?

LibreWind is an open project to design and document an affordable, resilient and reproducible wind instrument.

The initial idea is quite concrete:

- PP-R fusion pipe;
- common tools;
- a real mouthpiece;
- real reeds;
- tuning in C;
- simple fingering;
- dimensions suitable for children approximately 5 to 7 years old;
- and complete documentation so anyone can build it, study it and improve it.

We do not want to copy a saxophone.

Nor do we simply want to build a PVC flute that "sort of sounds right."

We want to **design an instrument**.

And we are going to document the process properly: measurements, calculations, mistakes, tests and decisions that did not work included.

# Why fusion pipe?

Because PP-R has one characteristic that is extremely useful for this project:

**it is hard to kill.**

It was designed for water installations, not for a developer with luthier delusions to use it for building musical instruments.

And that is exactly why we like it.

An instrument for children has to survive:

- bumps;
- drops;
- transportation;
- humidity;
- careless hands;
- and, eventually, being used as a sword.

Besides, the material is easy to find and can be worked with common tools.

The LibreWind philosophy is:

> **If you need a NASA laboratory to build it, it is not LibreWind.**

# Stage 0 - before making holes

The project's first rule was simple:

## Do not drill.

Measure first.

The commercial name of the pipe does not tell us enough to design it acoustically.

"Half-inch pipe" may be enough to buy it at a hardware store.

For us, we need to know:

- outside diameter;
- inside diameter;
- wall thickness;
- length;
- actual geometry;
- and how it interacts with the mouthpiece.

## Our small laboratory

![The LibreWind workbench](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-taller-inicio-proyecto.jpg)

*The Caña Libre laboratory. Tools, pipes, mouthpieces and a healthy amount of optimism.*

Here is our basic equipment:

- caliper;
- drill;
- bench grinder;
- drill bits;
- ruler;
- hand tools;
- PP-R pipe;
- mouthpieces;
- and a tuner on a phone.

Nothing too exotic.

# The pipe

The material selected for the first experiments is PP-R fusion pipe.

Our first measurement was approximately:

| Parameter | First measurement | Corrected measurement |
| --- | ---: | ---: |
| Outside diameter | 20.5 mm | **20.5 mm** |
| Inside diameter | 13.3 mm | **14.0 mm** |
| Calculated wall thickness | 3.6 mm | **3.25 mm** |

And here comes one of the project's first lessons.

### Measuring twice is not just a figure of speech.

The first inside-diameter measurement gave us 13.3 mm. When we repeated it more carefully, we discovered small particles or bits of residue on the edge of the pipe interfering with the caliper.

Also, during the fitting tests, we observed that a barb approximately 14 mm in diameter could enter the pipe **without excessive resistance, although it did need to be pushed**.

That made us question the first measurement.

We measured again correctly and obtained approximately:

**inside diameter = 14.0 mm**

The estimated wall thickness therefore becomes:

```text
e = (outside diameter - inside diameter) / 2

e = (20.5 - 14.0) / 2

e = 3.25 mm
```

We are not going to hide the first measurement.

We are leaving it documented because it is part of the experiment.

> **At LibreWind, mistakes are published too.**
>
> If a value was measured incorrectly, we correct it and explain why.

# Tools need to be measured too

Another early lesson was that saying "I have a 6.5 mm drill bit" does not necessarily mean that we actually have exactly 6.500 mm.

So we are going to measure the tools we use as well.

![Measuring a drill bit with a caliper](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-medicion-mecha-6-5-mm.jpg)

*The drill bit that says 6.5 mm also has to take an exam.*

![Drill bits and tools](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-mechas-taladro-herramientas.jpg)

*Our initial drilling arsenal.*

For future holes, we will prioritize drill-bit sizes that can actually be found in Argentina.

The strategy will be to start with small diameters and **enlarge them during tuning**, never the other way around.

Because:

> ### A drill bit only goes in once.
>
> ### The hole does not come back.

# The first big decision: alto or soprano?

Initially, we had two possibilities.

A generic alto saxophone mouthpiece and a Yamaha soprano mouthpiece.

After measuring them and observing how they related to our pipe, we decided that the **first experimental LibreWind would be model S, based on the soprano mouthpiece**.

The Yamaha mouthpiece is in good condition and uses a black Plasticover reed.

![Yamaha soprano mouthpiece](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-boquilla-yamaha-soprano.jpg)

*The protagonist of the first prototype.*

The alto mouthpiece is reserved for a second experimental line.

# Why start with soprano?

There are several reasons.

The first is ergonomics.

An instrument based on a soprano mouthpiece lets us investigate a potentially more compact geometry, which is especially interesting if the final goal is children aged 5 to 7.

The second is that the soprano mouthpiece's entry diameter is quite close to the inside diameter of our pipe.

The third is experimental:

**we first want to find out what this specific acoustic system can do.**

We do not want to design based on assumptions.

We want to measure.

# The adapter

And here we found our first small piece of engineering.

Instead of inventing a receiver from scratch, we used a half-inch hose barb fitting.

The barb enters approximately:

- **2 cm into the mouthpiece**;
- **2 cm into the pipe**.

To fit it inside the mouthpiece, we had to reduce its diameter by approximately 1 mm.

Since we do not have a lathe, the first adaptation was done by hand with the bench grinder.

![Adapting the barb for the soprano mouthpiece](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-adaptador-espiga-boquilla-soprano.jpg)

*First experimental adapter. It is not the final solution yet: it is a tool for learning.*

And here comes a small semantic clarification that, for Guybrush, is technically important.

The barb is not **"used."**

The barb is **pushed**.

Some force is needed to insert it into the pipe and the mouthpiece. It is not loose or rattling around, but we do not want to consider this fit final until we have checked:

- airtightness;
- mechanical strength;
- whether it can be disassembled;
- and, above all, that it does not damage the mouthpiece.

For now, it is an **experimental prototype**.

The final solution will probably be an adapter specifically designed for LibreWind.

# The first acoustic experiment

And this is where LibreWind really begins.

We still have:

- no holes;
- no fingering;
- no bell;
- no octave mechanism.

All we have is:

```text
Yamaha soprano mouthpiece
        +
adapter
        +
PP-R pipe
```

The goal is to find the length that makes the tube produce **concert C** with all future holes still nonexistent.

Our target is:

**C4 ≈ 261.63 Hz**

# First test: 45 cm

We cut a first long section, approximately **45 cm**.

The idea was deliberately conservative:

> a long pipe can keep being cut; a short one cannot be made longer again.

We did not make any holes.

With the soprano mouthpiece, adapter and test body, we obtained:

### **165 Hz**

That corresponds almost exactly to:

### **E3**

![First 45 cm LibreWind S prototype](https://chatgpt.com/assets/img/librewind/etapa-0/librewind-s-primer-prototipo-45cm.jpg)

*First experimental LibreWind S, still without holes.*

This is our first real acoustic data point.

# Second test: 30 cm

Then we cut the body down to approximately **30 cm**.

We attached exactly the same mouthpiece and adapter system again.

Result:

### **233 Hz**

The tuner indicated approximately:

### **A#3 / Bb3**

We now have two experimental data points:

| Body length | Measured frequency | Approximate note |
| ---: | ---: | --- |
| 45 cm | **165 Hz** | E3 |
| 30 cm | **233 Hz** | A#3 / Bb3 |
| - | **261.63 Hz** | **C4 - target** |

And this is much more interesting than a theoretical table found on the Internet.

Now we have **our own instrument as a laboratory**.

# How can we calculate the next cut?

Here physics enters the story.

For an ideal cylindrical tube closed at one end and open at the other, the fundamental frequency is approximately related to acoustic length by:

$$
f \propto \frac{1}{L}
$$

In other words: **the shorter the tube, the higher the note.**

But our instrument is not an ideal tube.

We have:

- mouthpiece;
- reed;
- mouthpiece chamber;
- barb;
- transition between diameters;
- acoustic corrections;
- and a tube with real-world dimensions.

So instead of imposing a perfect theoretical formula, we are going to build an approximation from **our own measurements**.

## Two points are already enough to build a first curve

We have:

```text
45 cm → 165 Hz
30 cm → 233 Hz
```

Let us first observe something interesting.

The length decreased by:

$$
\frac{45-30}{45}=33.3\%
$$

But the frequency increased by:

$$
\frac{233-165}{165}=41.2\%
$$

Therefore:

> **frequency does not change linearly with length.**

We cannot simply say:

> "each centimetre adds 4.5 Hz."

That would only be the average across this interval and would lead us into errors as we get closer to C.

# A simple mathematical approximation

We can represent our experimental data with a power function:

$$
f=aL^{-b}
$$

where:

- $f$ is frequency;
- $L$ is length;
- $a$ and $b$ are constants obtained from our measurements.

With the two points we have:

```text
L = 45 cm → f = 165 Hz
L = 30 cm → f = 233 Hz
```

we obtain approximately:

$$
b \approx 0.851
$$

and the experimental curve is approximately:

$$
f \approx 4212.5L^{-0.851}
$$

with $L$ expressed in centimetres.

There is no need to memorize this formula.

The important idea is something else:

> **we have a mathematical model built from the real behaviour of our instrument.**

# And where would our C be?

We are looking for:

$$
f=261.63\,\text{Hz}
$$

and solving the equation above.

The result is approximately:

# **L ≈ 26.2 cm**

That is our **first mathematical candidate**.

But be careful:

> **26.2 cm is not yet the final measurement for LibreWind.**
>
> It is a prediction.

We are going to check it with the real instrument.

# How much does the note change when we cut?

Here comes a very useful practical rule.

Between our first two experiments:

**45 → 30 cm**

produced:

**165 → 233 Hz**

But sensitivity increases near C.

According to our model, around 26 cm, removing approximately **1 cm** may represent several Hz of difference.

Therefore:

### Far from the target note

we can cut centimetres.

### Close to the target

we cut millimetres.

And when we are really close:

> **measure, cut very little, measure again.**

Never:

> "I think it needs just a tiny bit."

Because "a tiny bit" is a unit of measurement that has not yet made it into the International System of Units.

# An important observation

Physics gives us a prediction.

The tuner will give us reality.

That means the next episode does not begin by saying:

> "We have to cut it to exactly 26.2 cm."

It begins by saying:

> **"Our model predicts approximately 26.2 cm. Now we are going to see how wrong it was."**

And that is exactly what we want to learn throughout LibreWind.

# Project status

## Stage 0 - The laboratory

- Name: **LibreWind**
- Codename: **Caña Libre**
- Selected material: PP-R fusion pipe
- Measured outside diameter: **≈20.5 mm**
- Corrected inside diameter: **≈14.0 mm**
- Measurement error documented
- Tools identified
- Drill bits available
- Yamaha soprano mouthpiece selected
- Black Plasticover reed
- Experimental adapter built
- First test body
- 45 cm → **165 Hz**
- 30 cm → **233 Hz**
- First mathematical model
- Initial prediction for C4 → **≈26.2 cm**
- Verify C4 experimentally
- Design tone holes
- Design child-friendly fingering
- Get an octave
- Investigate a second register

# Next episode

## LibreWind S - Finding C

We start from:

**30 cm → 233 Hz**

Our model predicts:

**≈26.2 cm → C4**

But we are not going to cut directly down to that length.

We will approach it progressively and let the tuner tell us where our C really is.

Still:

**zero holes.**

First we need to find the acoustic heart of the instrument.

Then comes the truly dangerous part:

**making holes.**

And that is when we will need all the mathematics, acoustics and ergonomics we can gather.

Because:

> **a drill bit only goes in once.**
>
> **The hole does not come back.**

And if Guybrush finds himself facing a stubborn barb...

> **PUSH.**
>
> Never "use."
>
> Never.

🏴‍☠️🎷
