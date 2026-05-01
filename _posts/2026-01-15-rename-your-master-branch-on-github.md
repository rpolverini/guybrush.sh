---
layout: post
title: "Look behind you! A three-headed monkey! (And how to rename your master branch to main before the rubber chicken can squawk)"
date: 2026-01-15 11:59:00 -0300
categories: [git, devops, tutorial]
author: "Guybrush Threepwood"
lang: en
ref: rename-master-branch
---

Hey! I'm Guybrush Threepwood, and I want to be a DevOps engineer.

I've fought the ghost pirate LeChuck, I've held my breath underwater for ten minutes (for real!), and I've found the treasure of Big Whoop. But I swear nothing scared me quite like the first time I had to rename the `master` branch to `main` on a production repository. I was convinced the whole thing was going to blow up like the Scumm Bar on a rowdy night.

Turns out, just like a sword fight on the cliffs, it's not about brute force — it's about technique. Today I'll show you how to do it without your CI/CD pipeline sending you straight to Davey Jones' locker.

## Step 1: The Treasure Map (GitHub's web interface)

First things first, mate. Don't try to be a hero and do everything from the dark, damp console just yet. Head over to GitHub's website — it's easier than stealing the idol from Governor Marley while she sleeps.

1. Go to your repo and click on the **Settings** tab.
2. In the left menu, find **General** (usually the first option, hard to miss).
3. Scroll down a bit to **"Default branch"**.
4. You'll see the old `master` sitting there. Click the pencil icon ✏️ and rename it to `main`.

Bam! GitHub does the voodoo magic automatically: it redirects old links and moves your *Branch Protection Rules* on the spot. Magic, I tell you! Even the voodoo lady herself couldn't do a cleaner job.

## Step 2: The Sword Fight (Your local terminal)

Now, back to your ship (your laptop). Your local Git is going to be more lost than Stan trying to sell a used coffin in the middle of the ocean. You need to tell it who's in charge.

Open your terminal and hit it with these commands, one after another, like trading insults on the cliffs:

```bash
# 1. Rename your local branch (rename the ship)
git branch -m master main

# 2. Fetch updates from the old world (GitHub already knows main exists)
git fetch origin

# 3. Connect your local branch to the remote (align the cannons)
git branch -u origin/main main

# 4. Update the HEAD reference (fire!)
git remote set-head origin -a
```

And that's it! Your local repo is now in sync and shipshape.

## Step 3: Watch out for LeChuck's traps (CI/CD)

Hold your horses, deckhand! Don't celebrate just yet.

If you have any deployment pipelines, Dockerfiles, or wild scripts lying around, there's a solid chance they're still looking for `master`. And if they don't find it, your deploy is going to fail harder than my first attempt at pirate insults.

Check your config files (`.github/workflows`, `bitbucket-pipelines.yml`, or whatever you use) and make sure they say:

```yaml
on:
  push:
    branches:
      - main  # <--- CHANGE THIS, MATE
```

If you're using Vercel, Netlify, or any of those modern cloud tools, head to their dashboard and tell them the production branch is now `main`. Skip this step and you're crocodile bait.

---

Hey there, keyboard pirates!

Hope this saves your repos from aging worse than Stan's jacket. See? Renaming branches is dead simple once you know the trick — almost as easy as beating the arm wrestler at the shop. (Well, maybe not *that* easy, that guy had a grip.)

But don't get too comfortable. Check those pipelines, because if you break production on a Friday afternoon, I promise your Tech Lead is going to make LeChuck look like a friendly ghost.

See you in the next post! I'm off — Elaine's waiting for me for some mate and Grog at the Scumm Bar... or maybe she's sending me after Big Whoop's treasure again, you never know with her.

Oh, and never pay more than 20 bucks for a computer game! (Or for a .com domain.)
