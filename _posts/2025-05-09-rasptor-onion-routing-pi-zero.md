---
date: 2025-05-09 
layout: post
title: Rasptor - Routing All Raspberry Pi/Zero Traffic Through Tor
subtitle: A lightweight tool that forces all traffic from a Raspberry Pi/Zero device through the Tor network.
description: In this blog post, we will be looking at an open source tool that forces all traffic from a Raspberry Pi device through the Tor network. With a single command, Rasptor transparently routes all outgoing traffic — including DNS — through Tor’s anonymity layer.
image: https://images.unsplash.com/photo-1553406830-4fd6df625354?q=80&w=3432&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
optimized_image: https://images.unsplash.com/photo-1553406830-4fd6df625354?q=80&w=3432&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
category: blog
tags:
 - raspberry
 - pizero
 - tor
 - darkweb
 - anonimity
author: l4sec
paginate: true
---

> [https://github.com/L4ser-Security-Labs/rasptor](https://github.com/L4ser-Security-Labs/rasptor) 

**Rasptor** is a lightweight tool that forces all traffic from a Raspberry Pi device through the Tor network. With a single command, Rasptor transparently routes all outgoing traffic — including DNS — through Tor’s anonymity layer.

It’s designed for researchers, activists, hackers, and tinkerers who want **anonymity at the network level**, especially on compact Linux-based systems like the Raspberry Pi Zero 2 W.


### Why Rasptor?
The Raspberry Pi is a tiny powerhouse. But like any internet-connected device, it leaks data — through DNS requests, app-level calls, telemetry, and more. Even if you're using Tor Browser, many background services still talk directly to the internet.

**Rasptor solves this** by:
- Routing **all traffic** — not just browser traffic — through Tor  
- Ensuring **DNS requests** go through Tor’s DNSPort  
- Automatically handling **iptables rules**  
- Providing **one-liner control** with easy enable/disable/toggle  
- Rolling back cleanly if anything breaks  

It’s like turning your Pi into a stealthy ghost node 👻.


### Features
- Transparent Tor routing  
- DNS leak protection  
- Clean iptables setup and teardown  
- Auto Tor configuration and backup  
- Toggle between normal and Tor modes  
- Logging (start, end, uptime, actions)  
- Root permission check  
- Installable as a package with `rasptor` command  
- Reversible with zero system damage  


### Use Cases
- OSINT and threat intel collection (with better OPSEC)  
- Secure browsing from Pi kiosks  
- IoT deployments in adversarial environments  
- Personal anonymity projects  
- Educational labs for anonymity tools  


### How It Works (Under the Hood)
When you run `rasptor`:

1. It configures `/etc/tor/torrc` with:
    ```sh
    VirtualAddrNetworkIPv4 10.192.0.0/10
    AutomapHostsOnResolve 1
    TransPort 9040
    DNSPort 5353
    ```
2. It flushes and sets iptables rules:
    - Routes all TCP SYN packets to Tor’s TransPort (9040)  
    - Redirects all UDP DNS to Tor’s DNSPort (5353)  
    - Skips Tor’s own traffic and localhost  
3. It saves these rules via `netfilter-persistent`  
4. It runs `systemctl restart tor`  
5. It checks connectivity by hitting `https://check.torproject.org` via `curl`  

If anything goes wrong, you can roll back to normal mode.

### 🚀 Getting Started
Make sure you have tor and iptables installed:
``` sh
sudo apt update
sudo apt install tor iptables -y
```

Download the latest binary from the Release page and the run:
```sudo dpkg -i rasptor_1.0_all.deb```

Or download and install from Cloudsmith:

``` sh
wget https://dl.cloudsmith.io/public/l4ser-security-labs/rasptor/deb/any-distro/pool/any-version/main/r/ra/rasptor_1.0/rasptor_1.0_all.deb
sudo dpkg -i rasptor_1.0_all.deb
```

## 🚀 Usage
To enable Tor routing, run:
```sh
sudo rasptor
```

You’ll see a menu like:

```
=============================
       Rasptor Control
=============================
1) Enable Tor routing
2) Disable Tor routing
3) Toggle Tor routing
4) Exit
=============================
Choose an option [1-4]:
```

then select Enable Tor routing.

Disable Tor routing:
```sh
sudo rasptor
```
then select Disable Tor routing.
This restores default iptables rules and restarts your network stack.


Toggle routing (automatically switches based on Tor state):
```sh
sudo rasptor
```
then select Toggle Tor routing

## ❌ Uninstallation
```sudo dpkg -r rasptor```

## 🧪 Build from Source
``` sh
git clone https://github.com/l4sersec/rasptor.git
dpkg-deb --build rasptor
```

### 📊 Logs & Auditing
Rasptor logs every execution to `/var/log/rasptor/rasptor.log`, including:

- Start time  
- End time  
- Total uptime of Rasptor session  
- Action taken (enabled/disabled)  
- User that invoked the script  
- Output of Tor status and curl check  


### 🌐 Verifying You’re Using Tor
Once enabled, Rasptor runs:

```bash
curl --socks5-hostname 127.0.0.1:9050 https://check.torproject.org
```

You should see:

```
[✓] Success: System is routing traffic through Tor!
```

(Note: System-wide `curl` is used — not the browser.)


### 🗣️ Final Thoughts
Rasptor is built for the curious — those who want to explore how anonymity works at a low level, beyond the browser.

It’s not a magic bullet. You still need:

- Strong endpoint hygiene  
- No personal info leaking via apps  
- No browser fingerprinting  

But as a low-cost, high-impact anonymity layer for your Pi, Rasptor is a great start.

Pull requests, forks, and feedback welcome.

🔗 **[GitHub Repo](https://github.com/L4ser-Security-Labs/rasptor)**

---

**#rasptor #torproject #raspberrypi #privacytools #linux #infosec #cybersecurity**

