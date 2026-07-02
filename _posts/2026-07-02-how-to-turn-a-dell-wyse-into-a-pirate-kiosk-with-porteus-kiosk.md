---
layout: post
title: "🏴‍☠️ How to turn a Dell Wyse into a pirate kiosk with Porteus Kiosk"
date: 2026-07-02
categories: [linux, porteus, wyse, hardware]
tags: [porteus kiosk, wyse, dell, thin client, linux, chrome]
lang: en
ref: wyse-porteus-kiosk
---

# 🏴‍☠️ How to turn a Dell Wyse into a pirate kiosk with Porteus Kiosk

> "There are ships born to sail... and there are thin clients born to retire in a warehouse. We chose a different destiny."

If you've ever bought a **Dell Wyse** for almost nothing on Marketplace, you've probably had the same thought as me.

You look at it.

You plug it in.

You think:

> *"This has to be good for something more than collecting dust."*

And you were right.

With **Porteus Kiosk** you can turn that little box into an almost indestructible browser for digital signage, home automation, Home Assistant, control panels, reception desks, or any screen that just needs to open a browser and forget the rest.

---

# ☠️ Before we start

First, a clarification.

Wyse devices come in many versions (7010, 7020, etc.) and often even Dell doesn't seem to agree.

The easiest way to identify yours is by checking the CPU:

```bash
lscpu
```

or

```bash
cat /proc/cpuinfo | grep "model name"
```

With that you usually already know which model you really have.

---

# 🏴 Download Porteus Kiosk

The official site is:

https://porteus-kiosk.org/download.html

A few years ago there was a giant button that said **Download ISO**.

Now there isn't.

Welcome to the modern world.

To download the Community version you need to create a free account in the Customer Panel.

Once logged in, the system generates the ISO associated with your account and only then does the download link appear.

Download the **Porteus Kiosk Standard** version.

---

# ⚓ Preparing the disk

In my case I reused one of those things we all keep "just in case."

You know those USB external drives that died one day?

Well... inside they have a little SATA-USB board.

I took the board from a drive that had passed away and ended up using that adapter to connect the Wyse SATA module to my laptop.

Recycling at Monkey Island level.

Obviously, if you belong to the capitalist world, there are also SATA-USB adapters you can buy and they work exactly the same.

---

# 🦜 Identify the disk

Before connecting the disk we run:

```bash
lsblk
```

For example:

```text
NAME        SIZE
nvme0n1    512G
```

Now we connect the Wyse disk.

And we run exactly the same command again.

```bash
lsblk
```

Now something like this appears:

```text
NAME        SIZE
nvme0n1    512G
sdb          16G
```

Perfect!

Comparing both outputs makes it clear which one just showed up.

In my case it was:

```text
sdb
```

**VERY IMPORTANT**

We are going to use only the disk.

Not the partition.

So:

✔️ `/dev/sdb`

NOT

❌ `/dev/sdb1`

Because otherwise we would be writing inside a partition and not to the whole disk.

---

# ⚓ Unmount the disk

Ubuntu usually mounts it automatically.

Before flashing the image it is best to unmount it.

```bash
sudo umount /dev/sdb*
```

(replace the letter with the one that applies).

---

# 💣 Flash the image

Now comes the famous `dd`.

That command has a special skill.

It is incredibly fast...

...and incredibly capable of destroying the wrong disk.

Check the letter twice.

Then a third time.

And only then execute it.

```bash
sudo dd if=~/Downloads/Porteus-Kiosk-6.x.x-x86_64.iso of=/dev/sdb bs=4M status=progress
```

Where:

- `if=` → downloaded ISO file.
- `of=` → destination disk.
- `bs=4M` → copies much faster.
- `status=progress` → shows live progress.

For several minutes the terminal looks possessed while it copies blocks.

That's completely normal.

---

# ⚓ Wait until it's really done

When `dd` finishes there may still be data pending in the cache.

To force the system to write absolutely everything we run:

```bash
sync
```

When the prompt returns...

It's done.

You can remove the disk.

---

# 🚀 First boot

You reinstall the SATA module inside the Wyse.

You connect:

- keyboard
- monitor
- network (preferably Ethernet)

and turn on the machine.

After a few seconds the Porteus Kiosk wizard appears.

From there you configure:

- language
- network
- start URL
- resolution
- browser behavior
- updates

In less than ten minutes you have a kiosk up and running.

---

# ☠️ Why Porteus?

Because it does exactly one thing.

And it does it very well.

No desktop.

No users.

No Office.

No "what did the client touch now?".

There is a minimal Linux whose only job is to open a browser and keep working for years.

And that, for digital signage, Home Assistant, Grafana dashboards, receptions, or industrial systems, is worth gold.

---

# 🍺 Epilogue

While some people install twenty gigabytes of operating system just to open a web page...

...we recycled an office thin client with a disk rescued from an electronic corpse and left it ready to browse for years.

LeChuck would be proud.

Windows... well... is probably still looking for updates.

---

*"Because every technological problem can be solved with a terminal, a coffee, and the right amount of piracy."*
