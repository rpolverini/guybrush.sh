---
layout: post
title: "Build Your Own PaaS with Coolify on Oracle Cloud (Always Free)"
date: 2026-07-25
tags: [oracle-cloud, coolify, paas, docker, devops, infrastructure, monkey-island-vibes]
lang: en
ref: coolify-oracle-paas
---

# 🏴‍☠️ Prologue — Before Setting Sail

> *"We didn't build this ship to win a naval war. We built it to learn how to navigate."*
> — Guybrush Threepwood (probably)

If you've ever tried to deploy an application to the cloud, you've probably gone through at least one of these services:

- Render
- Railway
- Fly.io
- DigitalOcean
- Hetzner
- AWS
- Azure

They all work great... until the first bill arrives.

Oracle Cloud has a rather interesting quirk: their **Always Free** program offers resources that, if used wisely, are enough to set up a surprisingly powerful personal lab.

In this tutorial we'll use the **Ampere A1 ARM** instance, which for development and self-hosting typically offers excellent performance-to-power consumption ratio. Many modern projects (Docker, PostgreSQL, Redis, Nginx, Traefik, Python, Node.js, Go, Java, etc.) run perfectly on ARM and make excellent use of these resources.

For cases where Docker images still only exist for x86, we'll add a small AMD machine as a secondary *worker*. That way we get the best of both worlds.

The idea isn't just to install Coolify.

The idea is to understand how Oracle organizes its infrastructure:

- Compartments.
- Virtual Networks.
- Firewall rules.
- Instances.
- Storage.
- SSH keys.
- Security.
- ARM and x86 architectures.

Once you understand those pieces, deploying any other service becomes much easier.

---

# ☠️ Security Notice (very important)

This article is designed as a **learning lab**.

The goal is to have a deployment up and running in minutes to learn the platform and understand how all the components interact.

For that reason, we'll temporarily expose some services to the Internet, such as:

- Port **22** (SSH).
- Port **8000** (Coolify initial admin panel).

This **does not represent a recommended production configuration**.

From a cybersecurity perspective, any unnecessarily exposed service increases your attack surface. In other words...

> We're leaving the ship's door open while we learn where the cannons are.

In a real-world environment, it would be advisable, among other measures, to:

- Limit SSH access to known IP addresses.
- Disable password access and use SSH keys only.
- Close port 8000 after Coolify's initial setup is complete.
- Publish only HTTP/HTTPS behind Traefik.
- Apply the principle of **least privilege**, exposing only strictly necessary services.
- Keep the operating system updated.
- Implement monitoring, backups, and audit logs.
- Consider using security lists, Network Security Groups (NSG), VPN, or a *bastion host* for administrative access.

Throughout the blog we'll be hardening this installation step by step.

This is not the ship's final destination.

It's just the first port where we'll learn to navigate.

---

# 🍌 Guybrush Philosophy

In this series we don't look for perfect infrastructure from day one.

We look to understand what each piece does.

First we make the ship float.

Then we learn to navigate.

And only at the end... we worry about the pirates.

---

# 🏴‍☠️ Mission: Build Your Own PaaS with Coolify on Oracle Cloud (Always Free)

> *"While others pay 20 bucks a month for a VPS... we plunder Oracle's cloud using Always Free. Welcome to Monkey Island."*

After battling Oracle Cloud several times, I found a configuration that turned out really solid for running **Coolify** without spending a single doubloon.

The idea is to maximize the **Always Free Tier**, using the powerful Ampere ARM machine as the main server and, if any project needs x86 architecture, add a small AMD VM as a worker.

By the end of this tutorial you'll have your own PaaS, with automatic HTTPS, deployments from Git, and enough space to host several personal projects.

---

# ⚓ Resource Distribution

**Region**

📍 Chile Central (Santiago) — `sa-santiago-1`

## Main Server

| Resource | Value | Architecture |
|----------|-------|--------------|
| ARM Ampere A1 | — | — |
| CPU | 2 OCPU | ARM |
| RAM | 12 GB | ARM |
| Disk | 100 GB | ARM |

This is where Coolify will live.

---

## Optional Worker

| Resource | Value | Architecture |
|----------|-------|--------------|
| AMD x86 | — | — |
| CPU | 1/8 OCPU | x86 |
| RAM | 1 GB | x86 |
| Disk | 50 GB | x86 |

Ideal for containers that still don't have ARM images.

---

## Remaining Space

There are still approximately **50 GB** free out of the **200 GB** Oracle gives away.

Not bad for free.

---

# 🏝️ Step 1 — Create the Compartment

Log into Oracle Cloud and verify that the **Home Region** is:

> **Chile (Santiago)**

Then go to:

```
Identity & Security
    └── Compartments
```

Create a new one.

Name:

```
prod-coolify-compartment
```

Parent:

```
Root Tenancy
```

This compartment will let us keep the entire lab organized.

---

# 🌐 Step 2 — Create the Network

Go to:

```
Networking
    └── Virtual Cloud Networks
```

Select your compartment.

Choose:

```
Start VCN Wizard
```

Then:

```
VCN with Internet Connectivity
```

Name:

```
coolify-vcn
```

When the wizard finishes, go to:

```
Security Lists
```

and open the **Default Security List**.

---

## Open the Ports

Add these **Ingress** rules.

| Port | Use |
|------|-----|
| 22 | SSH |
| 80 | HTTP + Let's Encrypt |
| 443 | HTTPS |
| 8000 | Coolify initial admin panel |

**Important**

Oracle has a small detail that catches many people.

In the rules:

**DO NOT fill in "Source Port Range".**

It must remain completely empty.

The port goes only in:

```
Destination Port Range
```

---

# 💻 Step 3 — Create the ARM VM

Go to:

```
Compute
    └── Instances
        └── Create Instance
```

Name:

```
coolify-master-arm
```

Compartment:

```
prod-coolify-compartment
```

---

## Choose Ubuntu

Go to:

```
Change Image
```

Select:

```
Canonical Ubuntu
```

And explicitly choose an LTS version.

Recommended:

- Ubuntu 24.04 LTS
- Ubuntu 22.04 LTS

Architecture:

```
aarch64
```

---

## Choose the Right Machine

Go to:

```
Change Shape
```

Select:

```
VM.Standard.A1.Flex
```

Oracle hides a little secret here.

You have to expand the advanced configuration panel (the almost invisible arrow).

There you manually configure:

```
2 OCPU
12 GB RAM
```

Make sure it still shows:

```
Always Free Eligible
```

---

## Add Multiple SSH Keys

Choose:

```
Paste Public Keys
```

Paste them one below the other.

Example:

```
MacBook
Ubuntu Desktop
Notebook
```

As long as they're public keys (`*.pub`) Oracle will accept them all.

---

## Expand the Disk

In the **Boot Volume** section expand the advanced options.

Check:

```
Specify custom boot volume size
```

and enter:

```
100 GB
```

If we don't do this Oracle creates a much smaller disk and resizing it later is much more annoying.

Finally:

```
Create
```

---

# 🔥 Step 4 — The Hidden Ubuntu Firewall

This was the bug that cost me the most time.

Even though we open the ports in Oracle...

...Ubuntu can still block them.

Connect via SSH.

```bash
ssh ubuntu@PUBLIC_IP
```

Execute:

```bash
sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT

sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 443 -j ACCEPT

sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 8000 -j ACCEPT
```

Then save the rules.

```bash
sudo netfilter-persistent save
```

> **Captain's Note:** on some modern Ubuntu images you may need to install `iptables-persistent` first (`sudo apt install -y iptables-persistent netfilter-persistent`) if the save command doesn't exist.

---

# ⚙️ Step 5 — Install Coolify

Connected via SSH, simply run:

```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

Grab a coffee.

The installer does practically everything.

When it finishes, open:

```
http://PUBLIC_IP:8000
```

Create your admin user...

...and done.

Now you have your own Heroku, Render or Railway.

And free.

---

# 🖥️ Step 6 — Add an AMD Worker (Optional)

If you need to run Docker images that only exist for x86, create another VM.

Configuration:

```
VM.Standard.E2.1.Micro
Ubuntu 24.04
50 GB
```

Open the same ports using the procedure above.

Then from Coolify:

```
Servers

+ Add Server
```

Enter the public IP.

Coolify generates an SSH key.

Copy that key into:

```
~/.ssh/authorized_keys
```

on the AMD server.

Finally press:

```
Validate & Install
```

In a few minutes Coolify automatically installs Docker and leaves the node ready to receive deployments.

---

# 🏴‍☠️ Conclusion

With this architecture we get:

- ✅ An ARM server with **2 OCPU and 12 GB RAM**.
- ✅ An x86 worker for ARM-incompatible projects.
- ✅ Automatic HTTPS via Traefik and Let's Encrypt.
- ✅ Deployments from GitHub.
- ✅ Docker managed from a web interface.
- ✅ All within Oracle Cloud's **Always Free** plan.

Not bad for an infrastructure that costs exactly **0 doubloons per month**.

In the next episode we'll see how to connect your own domain, configure DNS, and have Coolify publishing applications as if we had a small hidden datacenter on Monkey Island.
