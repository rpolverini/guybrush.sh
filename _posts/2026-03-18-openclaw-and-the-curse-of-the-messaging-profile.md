---
layout: post
title: "OpenClaw and the curse of the messaging profile"
date: 2026-03-18
tags: [openclaw, devops, virtualbox, sandbox, monkey-island-vibes]
lang: en
ref: openclaw-messaging-curse
---

There are days when you wake up ready to conquer the digital Caribbean... and others when you decide to install OpenClaw v2026.3.2 on a shielded machine inside VirtualBox. I, clearly, chose the second. A tactical blunder worthy of a pirate trying to open a door by pushing on the "pull" side.

It all started when Governor Marley — a kind of Elaine but with RFCs under her arm — decided to lay down the law on the island. "Order, security, sandboxing," she said. And I, as naive as Guybrush walking into the Scumm Bar for the first time, thought: _"great, this is going to work perfectly"_.

Spoiler: no.

## 🏴‍☠️ The cursed profile: "messaging"

I install OpenClaw, fire it up... and the thing behaves like a trained parrot:

- talks ✔️  
- responds ✔️  
- does anything useful ❌  

That's when you discover the most elegant trap since the insult sword fight: the `messaging` profile.

This mode is basically like hiring a mercenary who only gives you motivational advice. It's designed for "security by default," which in plain English means:

> "I won't let you break anything... but I won't let you do anything either."

The agent cannot:
- execute commands  
- touch files  
- or even glance sideways at your system  

It's like having LeChuck locked in a bottle... but without the magic. Just useless.

## 🧱 The sandbox: the island within the island

When you think you've understood the problem, the second boss appears: the sandbox.

OpenClaw decides that every session lives inside its own mini-world:
- Docker  
- OpenShell  
- ghost filesystem  

You tell it:
> "hey, go to `/home/ramiro/proyecto`"

And the agent looks at you like Guybrush when you bring up quantum physics:
> "I don't know that place, mate."

Because of course — it's locked in its little virtual island. No mounts, no binds, nothing. A Robinson Crusoe without Friday.

## ⚙️ The ritual to restore its soul

After swearing for a bit (mandatory phase), you find the right spells:

```bash
openclaw config set tools.profile full
openclaw gateway restart
```

Only then does the thing start behaving like a proper code pirate.

But it doesn't end there. Because OpenClaw is like an old ship: every sail has to be hoisted by hand:

```bash
openclaw config set tools.allow_shell true
openclaw config set tools.allow_filesystem true
openclaw config set tools.allow_network true
openclaw config set tools.allow_exec true
```

And finally, you mark the territory:

```bash
openclaw config set agents.defaults.workspace "~/.openclaw/workspace"
```

Now we're talking. Now the agent stops being a decorative NPC and becomes actual crew.

## 🪟 Bonus track: the ghost of Windows

Throughout this whole process, one thing became crystal clear:  
if you're trying this on Windows... you're playing on nightmare mode.

Not my words — experience talking:  
it's like trying to sail the Caribbean in a punctured inflatable boat.

## 🧠 Conclusion: the real enemy wasn't LeChuck

You think the problem is virtualization, or Docker, or permissions...  
but no.

The real villain was that tiny little detail hidden in the config:  
`tools.profile = messaging`

One line. One switch. One curse.

And that's how, while trying to install OpenClaw in a VM more armored than a pirate's treasure chest, I learned that sometimes the problem isn't a lack of power...

...but that someone very kindly decided you shouldn't have any.

---

And remember: in the world of software, as in Monkey Island, never trust something that works "by default"... unless you want to end up fighting with a rubber chicken on a pulley.
