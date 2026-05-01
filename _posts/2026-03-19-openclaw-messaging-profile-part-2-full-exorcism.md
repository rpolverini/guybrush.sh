---
layout: post
title: "OpenClaw and the curse of the messaging profile (part 2: full exorcism)"
date: 2026-03-19
tags: [openclaw, devops, virtualbox, sandbox, debugging, monkey-island-vibes]
lang: en
ref: openclaw-messaging-curse-2
---

If you thought the story ended with the `messaging` profile...

let me tell you something, comrade:

**we were barely out of the tutorial.**

---

## 🪦 Still broken (and now it was personal)

After:

- switching profiles  
- wrestling with the sandbox  
- invoking forgotten commands  
- suspecting Docker that wasn't even installed  

OpenClaw was still the same:

> talked smooth... but didn't touch a thing.

No filesystem, no shell, nothing.

It was like having Guybrush with all the attitude...  
but tied to a chair.

---

## 🧠 Plot twist: it wasn't config... it was a broken install

That's when it clicked:

👉 it wasn't misconfigured  
👉 **it was broken from the very beginning**

The one-liner had left me with a weird hybrid:
- no persistent config  
- no complete commands  
- no real capabilities  

A decorative OpenClaw.

---

## 🧹 The exorcism (scorched earth)

No patch is going to fix this.

This was a **total wipe — "burn the island and start over" level**:

### 🧨 Step 1 — Stop & Remove Everything

```bash
# Stop gateway
openclaw gateway stop || true

# Uninstall
npm uninstall -g openclaw

# Clean up the leftovers
rm -f /usr/bin/openclaw
rm -rf /usr/lib/node_modules/openclaw

# Wipe ALL state
rm -rf ~/.openclaw ~/.clawdbot ~/.moltbot ~/.molthub
```

And the final blow to the background service:

```bash
# Linux
systemctl --user disable --now openclaw-gateway.service
rm -f ~/.config/systemd/user/openclaw-gateway.service
systemctl --user daemon-reload
```

---

## 🔍 Step 2 — Verify nothing's left behind

```bash
command -v openclaw || echo "Binary removed ✓"
ls ~/.openclaw 2>/dev/null || echo "State dir removed ✓"
```

👉 If anything's still there... **the ritual isn't done yet.**

---

## 📦 Step 3 — Clean install (the real deal)

With Node 22+:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

or:

```bash
npm install -g openclaw@2026.3.13
```

---

## 🚀 Step 4 — Onboarding like a civilized person

```bash
openclaw onboard
```

This:
- creates real config  
- sets up the workspace  
- installs the gateway  
- defines skills correctly  

Then:

```bash
openclaw doctor
openclaw status
```

---

## ⚡ Result (the good part)

And here's the important bit:

👉 **without touching a single config manually**

OpenClaw started:

- ✔ accessing the filesystem  
- ✔ writing files  
- ✔ using the shell  
- ✔ working like it should  

From the workspace... all the way to disk.

---

## 🧠 Technical lesson

In OpenClaw 2026:

> **if something doesn't work, don't assume config... assume corrupt install.**

Because:
- the system is now declarative  
- onboarding sets everything up  
- and editing JSON by hand is like sword fighting with a rubber chicken

---

## 🏴‍☠️ Final conclusion

After hours debugging sandbox, profiles, and permissions...

the real problem was simple:

👉 **I had summoned the demon... with a badly drawn circle.**

---

And remember, code pirate:

> sometimes you don't need to fix the ship...  
> you need to sink it and build a new one.

---

As a wise old sage of the digital Caribbean once said:

**"Never trust an install that 'sort of' works... that ends up worse than running Windows on the high seas."**
