---
layout: post
title: "🏠 How to install Home Assistant OS on a Dell Wyse"
date: 2026-09-20
categories: [linux, home assistant, wyse, hardware, home automation]
tags: [home assistant, haos, wyse, dell, thin client, linux, home automation, uefi]
lang: en
ref: wyse-home-assistant
---

# 🏠 How to install Home Assistant OS on a Dell Wyse

> "Every pirate needs a home port. Somewhere small, ugly on the outside, that never shuts down and always knows where everything is."

A few months ago we [turned a Dell Wyse into a pirate kiosk with Porteus Kiosk]({% post_url 2026-07-02-how-to-turn-a-dell-wyse-into-a-pirate-kiosk-with-porteus-kiosk %}).

In that post I mentioned Home Assistant as a use case at least three times.

And always the same way: as *the screen that displays Home Assistant*.

Never as **the server that runs Home Assistant**.

Today we fix that.

Because if there's one machine built for home automation, it's exactly this one: no fan, drawing ten or fifteen watts, plugged into a corner, running 365 days a year.

A Raspberry Pi costs more, has less RAM, and boots from a microSD card that one day — always, inevitably, on a Sunday — is going to corrupt itself.

The Wyse, on the other hand, already came with a real disk.

---

# ☠️ Before we start: the UEFI fine print

Here's the filter that decides whether this post is useful to you at all.

Worth reading **before** you download two gigs of nothing.

Home Assistant OS has one non-negotiable requirement:

> The system must be 64-bit capable and be able to boot using **UEFI**.

Legacy BIOS is not supported. It's not "works but with some hassle". It simply does not boot.

And this matters a lot, because Wyse boxes split fairly cleanly into two camps:

**The ones that will:**

- Wyse 3040 (Atom x5)
- Wyse 5070 (Celeron / Pentium Silver)
- Any Wyse with UEFI firmware in its setup

**The ones that probably won't:**

- Wyse 5010 / 7010 (AMD G-series, the classic Marketplace find)
- Earlier models with legacy BIOS only

Before doing anything, get into the Wyse setup (`F2`, sometimes `Del`) and look for something like **Boot Mode**, **Boot List Option** or **UEFI Boot**.

If you can pick UEFI: keep reading, this post is for you.

If only *Legacy* exists: don't throw the machine away, skip straight to [Plan B](#-plan-b-if-your-wyse-is-legacy-bios) at the end.

---

# 🦜 The other detail: the disk

The second place where people smash their nose.

In the Porteus post I used a **16 GB** SATA module and had space to spare, because Porteus doesn't care about anything: it loads into RAM and that's it.

Home Assistant OS is a different story.

HAOS is a full operating system with duplicated partitions for A/B updates, plus Docker, plus the add-ons, plus the database that grows **every single day** with every sensor you record.

The image fits in 16 GB.

Installing it isn't the problem. The problem is month three, when the database has eaten the disk and you start deleting history just to breathe.

Practical recommendation, from experience rather than the manual:

- **32 GB** → reasonable minimum
- **64 GB or more** → what you actually want
- A cheap 2.5" SSD → what you'll probably end up fitting

The SATA modules in Wyse boxes are replaceable, and 64 GB ones go for pocket change.

One more detail, because it's the kind of thing that costs you a whole afternoon: the disk has to use **512-byte logical sectors** (512n or 512e). Native 4Kn drives won't boot HAOS. Any normal consumer SSD is fine; this applies to odd enterprise drives.

---

# ⚓ Identifying your Wyse

Same as last time, the most honest way to know what you bought is to look at the CPU.

If you have any Linux booted on it:

```bash
lscpu
```

or

```bash
cat /proc/cpuinfo | grep "model name"
```

An `AMD G-T56N` is politely telling you that you're heading straight for Plan B.

A `Celeron J4105` or an `Atom x5-Z8350` means you're in the race.

---

# 🏴 Downloading Home Assistant OS

No account creation this time. After the Porteus odyssey, this feels almost like a caress.

The image lives on GitHub, in plain sight:

https://github.com/home-assistant/operating-system/releases

Find the release marked **Latest** and download the file that says `generic-x86-64`.

At the time of writing, the stable version is **18.3**:

```bash
wget https://github.com/home-assistant/operating-system/releases/download/18.3/haos_generic-x86-64-18.3.img.xz
```

That's about 500 MB compressed.

**Don't extract it yet.** I'll explain why later.

Before moving on, check the file arrived intact. The `sha256` is on the release page:

```bash
sha256sum haos_generic-x86-64-18.3.img.xz
```

Compare the first and last few characters against what GitHub says, and you're done.

Two seconds now saves you an hour of "why won't it boot".

---

# 💣 Writing the image

This is where the official manual and I part ways.

The Home Assistant documentation recommends using Ubuntu's **Disks** utility or **Balena Etcher**. That's the safe route, with a GUI and big buttons.

If you've never written an image to a disk in your life, do it that way. Seriously. Grab Etcher, hit *Flash from file*, pick the disk, done.

But this is a pirate blog, and we already have the SATA module dangling off a USB adapter — the same one I stole from the corpse of an external drive in the previous post.

So we're going with the terminal.

## Identifying the disk

The usual trick. Before plugging anything in:

```bash
lsblk
```

```text
NAME        SIZE
nvme0n1    512G
```

Plug in the Wyse disk. Same command again:

```bash
lsblk
```

```text
NAME        SIZE
nvme0n1    512G
sdb         64G
```

The one that just appeared is ours.

**VERY IMPORTANT**, and I'm repeating it because I repeated it last time and I'll repeat it forever:

We use the **drive**, not the partition.

✔️ `/dev/sdb`

❌ `/dev/sdb1`

## Unmounting

Ubuntu mounts everything it finds, with an enthusiasm that's occasionally annoying.

```bash
sudo umount /dev/sdb*
```

If it says it wasn't mounted, even better.

## Writing

And here's why we didn't extract the `.xz` earlier: there's no need. We decompress it on the fly and feed it to `dd` through a pipe.

```bash
xzcat haos_generic-x86-64-18.3.img.xz | sudo dd of=/dev/sdb bs=4M status=progress
```

Where:

- `xzcat` → decompresses the image without creating a multi-gig intermediate file.
- `of=` → the target disk. **Look at it three times.**
- `bs=4M` → big blocks, copies much faster.
- `status=progress` → so the terminal tells you what's going on.

`dd` still has the same talent it always had: it's blazing fast, and it's perfectly capable of annihilating the wrong disk without asking anything or apologising afterwards.

Check the letter. Then check it again. Only then hit enter.

## Waiting for real

When `dd` finishes, there may still be data floating in the cache.

```bash
sync
```

When the prompt comes back, you're done. You can unplug the disk.

---

# 🔧 Preparing the BIOS

Put the SATA module back inside the Wyse and get into the setup (`F2`).

Three things, not one more:

1. **Boot Mode → UEFI**. Without this, nothing above mattered.
2. **Secure Boot → Disabled**. HAOS isn't signed for Secure Boot.
3. **Boot order** → internal disk first.

And a fourth, optional but strongly recommended if this is going to be your home automation server: look for something like **AC Recovery**, **Restore on AC Power Loss** or **After Power Failure** and set it to **Power On**.

That way, when the power drops and comes back, the Wyse boots on its own.

A home automation server that needs someone to walk over and press its button isn't a server. It's a pet.

Save, exit.

---

# 🚀 First boot

Connect:

- monitor and keyboard (just for this first time)
- **network with internet access** — this is not optional
- power

The network genuinely matters on first boot: HAOS ships with the operating system, but it **downloads Home Assistant Core from the internet** the first time it starts.

With no network, the screen just stares back at you and nothing happens.

Ethernet is ideal. But if you don't have a cable handy — because the router is on the roof, because it's raining, because that's life — don't let it stop you: [you can configure WiFi from the console](#-configuring-wifi-from-the-console) and the step-by-step is further down.

Power it on.

After a minute a welcome banner shows up on the screen. That means the base system booted fine.

And then... you wait.

The first time takes a while. Several minutes. It's pulling containers and building everything from scratch.

It's a strange moment, with the screen almost frozen, where you start doubting every decision you made that day.

Hang in there. It's working.

---

# 📡 Configuring WiFi from the console

If you started with a cable, skip this whole section.

If not — and this is far more common than tutorials admit — here's the complete path. All of this happens on the Wyse console, with the keyboard plugged into it, no SSH or anything.

## The `ha >` prompt and the prefix trap

Once the console tells you the Supervisor is running, press **Enter**. This shows up:

```text
ha >
```

And here's the first trap, the one that costs everybody half an hour:

**At this prompt you do NOT type `ha` in front.**

All the official documentation shows commands as `ha network info`, because it assumes you're running them over SSH or from the Terminal add-on. On the local console you're already *inside* the CLI, so it's just:

```text
ha > network info
```

If you type `ha network info` there, it'll tell you it doesn't know that command. And you'll swear the CLI is broken.

Further down we'll see that when you drop into a real shell, the prefix **comes back**. It's exactly inverted. Keep it in mind.

## Checking network state

```text
ha > network info
```

And here's the second practical problem: that output is a huge dump, and **there's no reliable scrollback on the real console**. The information you care about scrolls up and never comes back.

You can try **`Shift + PageUp`** / **`Shift + PageDown`**, which moves the video buffer on the Linux console. On some Wyse boxes it works; on others the firmware doesn't support it and nothing happens.

The real fix is asking for just the interface you care about, because `network info` takes an optional argument:

```text
ha > network info wlp5s0u1
```

That fits on one screen and you're done. You won't find this in any tutorial, but it's been in the CLI forever.

Your WiFi interface name will be something like `wlp5s0u1` or `wlan0`. If it shows up in the System Information at boot, the kernel already loaded the driver: good sign, the USB adapter is supported.

## `enabled: false` is normal. Don't go looking for how to enable it.

When you check the state, you'll most likely see this:

```text
connected: false
enabled: false
```

And the natural reaction is to go hunting for a command to enable the interface.

**There isn't one, and you don't need it.**

That `false` doesn't mean "broken" or "deliberately disabled". It means **"not configured yet"**: HAOS leaves WiFi asleep until you give it a network to associate with.

And the best part: the same command that configures the network enables it on its own. Looking at the CLI source, the `--disabled` flag defaults to `false`, and the inverted value gets sent on **every** `network update` call. Which means each update says `enabled: true` without you doing a thing.

One call, three things solved: enable, store credentials, request an IP.

## Seeing what networks are around

```text
ha > network scan wlp5s0u1
```

This step is worth it because it confirms the adapter **actually** sees networks, before you start fighting with the password.

If it lists your SSIDs, the chipset works and we've already won.

If it errors out, don't panic: with the interface still disabled, the scan sometimes fails. Skip it and go straight to the update.

## Connecting

```text
ha > network update wlp5s0u1 --ipv4-method auto --wifi-mode infrastructure --wifi-auth wpa-psk --wifi-ssid "MyNetwork" --wifi-psk "MyPassword"
```

Swap `wlp5s0u1` for your interface, and the SSID and password for yours.

About the values:

- `--wifi-auth wpa-psk` covers both WPA2 and WPA3-personal. For an open network it's `open` and you drop the `--wifi-psk`.
- `--wifi-mode infrastructure` is the normal client mode. The API defaults auth to `open`, so it's worth passing everything explicitly.
- **Always keep the quotes**, especially if the password contains spaces, `$`, `!` or `#`.

It takes a few seconds and doesn't always print anything useful. Silence here is a good sign.

## Verifying

```text
ha > network info wlp5s0u1
```

It should now say `connected: true` and show an IP where it previously said `no addresses`.

Write it down. That's your Home Assistant address.

## If it doesn't get an IP

In order of likelihood:

- **Wrong band.** The network is 5 GHz and the USB adapter only does 2.4 (or the other way round). If the router broadcasts both bands under the same name, try splitting them.
- **Mistyped password.** There's no command to fix just the password: run the whole `network update` again.
- **Hidden SSID.** It won't show up in the `scan`, but the `update` works anyway if the name is exact.
- **Odd characters in the password** that the parser chokes on.

## When you need a real shell

If at some point you want to grep an output, save it to a file, or look at `dmesg`, the `ha >` prompt won't cut it: it isn't a shell. No pipes, no redirection, no `grep`. If you write `network info > /tmp/something.txt`, the `>` gets read as one more argument and it fails.

For that, from the local console:

```text
ha > login
```

That drops you into a shell on the host. It doesn't ask for a password because you're physically in front of the machine — and that's precisely why it only works on the local console and not over SSH.

And here the prefix we dropped at the beginning comes back:

```bash
ha network info > /tmp/net.txt
cat /tmp/net.txt
dmesg | grep -i wlp
```

Inside the shell it **does** take `ha` in front. It's the exact inversion of the previous prompt, and it's the single most confusing thing about the whole system.

---

# 🧭 Getting into Home Assistant

From any computer on the same network:

http://homeassistant.local

Watch out for this one, because almost every tutorial gets it wrong: HAOS serves the interface on **port 80**, so you don't need to add anything.

The famous `:8123` is the **fallback**, for when port 80 is taken (a reverse proxy, for example). If `homeassistant.local` doesn't answer, try in this order:

- http://homeassistant.local:8123
- http://homeassistant
- `http://THE.WYSE.IP.HERE`

To find the IP: the `network info` from the previous section, the console banner, or your router's DHCP list looking for the new client called `homeassistant`.

## The Observer, which nobody tells you about

There's a second address, and it's the most useful one when things are half-working:

http://homeassistant.local:4357

That's the **Observer**, a small independent service that tells you what the Supervisor is doing while it starts up.

It helps at exactly the worst moment: when the installation is halfway there, the main interface still isn't answering, and you don't know whether the system is working or hung.

If the Observer answers but port 80 doesn't, the system is alive and still downloading. Be patient.

If neither answers, then you really do have a network or boot problem.

## Onboarding

Once it loads, onboarding asks you for:

- admin username and password
- installation name
- location and timezone
- whether you want to share anonymous statistics

And then Home Assistant goes out and looks for devices on your network by itself.

The first time is a nice moment. Things show up that you didn't even know were connected: the Chromecast, the printer, the TV, some Shelly, a stray light bulb.

Your house just sent you its inventory.

---

# 🩹 If it doesn't boot

It happens. Especially on stubborn firmware that doesn't register the new disk's UEFI boot entry.

The Wyse stares at you with a "I can't find anything bootable" face and you swear `dd` failed.

It didn't. The image is right there; what's missing is the firmware knowing it exists.

Boot a live Linux from USB (in UEFI mode) and tell it by hand:

```bash
sudo efibootmgr --create --disk /dev/sda --part 1 --label "HAOS" \
   --loader '\EFI\BOOT\bootx64.efi'
```

Replacing `/dev/sda` with the disk where you wrote HAOS — careful, here the name is the one it has **inside the Wyse**, which isn't necessarily the same one it had hanging off USB on your laptop.

Some BIOSes also let you add the boot option manually, pointing at:

```text
\EFI\BOOT\bootx64.efi
```

Reboot and you're in.

---

# ⚙️ The three things to do right away

Before you start playing with automations and dashboards, do these three. It's fifteen minutes and it saves you future pain.

**1. Static IP.** Reserve the Wyse's IP in your router's DHCP. Everything you integrate later will point at that address, and the day the router decides to change it, half your system breaks. If you connected over WiFi, this matters twice as much.

**2. Automatic backups.** Under *Settings → System → Backups*. Schedule them and send them off the Wyse: a NAS, Google Drive via add-on, whatever you have. A backup stored on the same disk that can die isn't a backup, it's a wish.

**3. Take the monitor away.** You don't need it anymore. The Wyse boots on its own, without keyboard or screen, and the network configuration is saved. Plug it into the corner where it's going to live and forget about it.

And if you want to make use of that now-free USB port: a **Zigbee** dongle (a Sonoff ZBDongle-E, for instance) turns this into a full home automation hub, with no cloud and no dependency on anybody's app.

But that's another post.

---

# 🛠 Plan B: if your Wyse is legacy BIOS

If you got this far and your Wyse has no UEFI, don't throw it out.

Home Assistant OS is off the table, yes. But Home Assistant isn't.

The path is: **minimal Debian 12 + Home Assistant in Docker**.

Debian boots fine on legacy BIOS, weighs almost nothing, and Home Assistant runs just the same in a container:

```yaml
services:
  homeassistant:
    image: ghcr.io/home-assistant/home-assistant:stable
    container_name: homeassistant
    volumes:
      - ./config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
```

What you lose is the Supervisor, and with it the one-click add-on store.

What you gain is being able to run Mosquitto, Zigbee2MQTT or Frigate alongside it as your own containers, and the whole system fits comfortably in 16 GB.

You trade convenience for control. Which is, basically, the decision you make every time you open a terminal.

---

# 🍺 Epilogue

A thin client some company retired as obsolete is now the brain of a house.

No fan. No cloud. No subscription. Nobody on the other side of the world knowing what time you turn your lights off.

Fifteen watts doing the job the industry wants to charge you monthly for.

Guybrush never bought a complete map: he collected three loose pieces and found the treasure with that.

We collected a Marketplace Wyse, a salvaged SSD, a GitHub image, and a USB WiFi adapter that was rattling around in a drawer — because it was raining and an ethernet cable wasn't an option.

Windows, meanwhile, is still checking for updates.

---

*"Because every technical problem can be solved with a terminal, a coffee, and just the right amount of piracy."*
