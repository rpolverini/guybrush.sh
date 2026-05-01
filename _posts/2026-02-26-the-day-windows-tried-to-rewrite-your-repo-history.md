---
layout: post
title: "The day Windows tried to rewrite your entire repo history"
date: 2026-02-25
categories: [git, devops, adventures]
lang: en
ref: windows-crlf-history
---

There are dangerous enemies in software development.

- Intermittent bugs.
- Friday deploys.
- And then... Windows and its line endings.

Today we're talking about one of the classic naval battles of cross-platform development:

> "Why is Git saying every single file was modified when nobody touched anything?"

Spoiler: it wasn't black magic. It was `CRLF`.

---

## 🏝️ The invisible battle: LF vs CRLF

On civilized systems (Linux, macOS), a line ending is:

```text
LF → \n
```

Minimalist. Elegant. Like a well-written Python script.

On Windows, however:

```text
CRLF → \r\n
```

Two characters. Because apparently one just wasn't dramatic enough.

So what happens?

1. You save files with `LF` on Linux.
2. Your colleague opens the file on Windows.
3. Windows says: "This needs more drama" and converts it to `CRLF`.
4. They commit.
5. Git sees that EVERY single line changed.
6. The diff looks like the Scumm Bar after a brawl.

And there you are, staring at 400 "modified" lines where nobody changed a thing.

---

## 🧠 Why it doesn't happen in other projects

Because those projects already have the ancient blessing:

`.gitattributes`

That file is basically the colonial governor telling Git:

> "Everything gets stored in LF here. Order and discipline."

---

## 🛡️ The fix — like true senior pirates

### 1️⃣ Create `.gitattributes` at the repo root

```bash
touch .gitattributes
```

And inside it:

```text
* text=auto
```

What does this do?

- Normalizes everything to `LF` on commit.
- Lets Windows use `CRLF` locally if it wants.
- But keeps the repo clean and consistent.

It's like saying:

> "Do whatever you want in your quarters, but on this ship, I'm captain."

---

### 2️⃣ Recommended config per system

Your Windows colleague:

```bash
git config --global core.autocrlf true
```

You on Linux:

```bash
git config --global core.autocrlf input
```

Pirate translation:

- Windows converts on checkout.
- Linux doesn't touch anything on checkout.
- Both convert to `LF` on push.

World peace.

---

## 🧹 What if it already blew up?

If there's already a commit where "everything changed":

1. Make sure you have no pending changes (`git status` clean).
2. Run:

```bash
git add --renormalize .
```

3. Corrective commit:

```bash
git commit -m "Normalizing line endings to LF"
```

Done. The repo stops looking like it was ransacked by incompetent corsairs.

---

## 🧱 The solid version (for teams past training wheels)

If you want something more explicit and professional:

```text
* text=auto eol=lf
```

And to keep Git from touching binaries:

```text
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.pdf binary
```

Because the last thing we want is Git trying to "normalize" an image like it's a poem.

---

## ⚔️ Final lesson

Conflicts between operating systems aren't technical.

They're cultural.

Linux believes in simplicity.  
Windows believes in emotional over-engineering.

But Git, like a good captain, needs clear rules.

If you don't have `.gitattributes`, you don't have civilization.

---

Next time you see a diff where everything changed and nothing changed...

Don't blame your colleague.  
Blame the carriage return.

---

And remember:

> A true pirate doesn't fear merges...  
> they fear misconfigured line endings.
