---
title: "I Tried the Viral Clawdbot - Here's a Privacy-First Local Setup"
date: 2026-01-29
categories:
  - AI
  - Self-Hosting
tags:
  - Moltbot
  - Ollama
  - Local AI
  - Privacy
  - Windows
  - Linux
excerpt: "The setup friction was real - five distinct problems across two machines - but they're all solvable, and once solved, they tend to stay solved."
classes: wide
---

If you've been online lately, you've probably seen **Clawdbot** popping up everywhere - this quirky little "agent that actually does things." Then, almost immediately, it was renamed (it's **Moltbot** now).

The hype is entertaining - but what pulled me in was the *architecture*. Moltbot is built around a **local gateway**: something you run yourself that bridges messages, tools, and an agent runtime. That's a fundamentally different direction than "yet another chatbot tab."

And here's my real motivation: I want to use AI for the personal stuff that actually matters - home admin, family logistics, finances, notes, the boring-but-sensitive life tasks. Those are also the exact categories I'm least comfortable sending to AI Labs by default. I'm not trying to replace frontier AI agents for everything. I *am* trying to build a setup where the **privacy-sensitive lane stays on my network**.

Micro-truth: local isn't a flex here - it's a boundary.

That's where the hardware changes the story. (Ubuntu on a **North XL** build: **RTX 5090**, **Ryzen 9 9950X**, **96GB RAM**, **990 Pro NVMe** storage - 2TB + 4TB.) It's enough horsepower that local inference feels like infrastructure - meaning I can keep the private stuff on-prem *and* still have it run like a real assistant.

The promise was simple:

> install Moltbot -> point it at Ollama -> done.

The reality took an hour of debugging across two machines and multiple moving parts.

Here's what actually happened - so you don't have to repeat it.

---

## The Dream

I needed this to work **from Windows** for a simple reason: that's where my day-to-day life happens.

For anyone wondering: if the local model only worked when I SSH'd into the server, it wouldn't become part of my routine. My "life admin" artifacts I want an assistant to help with - those live on my Windows box or accessible from one (BitLocker encrypted connected drives). If the local model only worked when I SSH'd into the server, it wouldn't become part of my routine. The whole goal was: keep the sensitive stuff private **and** keep it usable where I actually do the work.

So the target workflow was straightforward: type a prompt on my Windows machine and have it answered by a 30B-ish model running on my server - private, fast-ish, and local-only.

The architecture is straightforward:

1. **Ollama** runs the model
2. **Moltbot** provides the interface/agent runtime
3. **Moltbot Gateway** bridges client <-> server across the network

Three pieces, one goal.

---

## The First Ten Minutes: False Confidence

Server setup went smoothly:

- Ollama installed in seconds
- Pulled a model
- Installed Moltbot
- Started the gateway
- Tested locally - it worked

"This is easy," I thought.

Then I tried connecting from my Windows machine.

---

## The Config File That Wasn't

I added the gateway URL to my config. Ran Moltbot. It tried connecting to **localhost**.

*Localhost?* I just told it to use my server's IP.

Turns out Moltbot on Windows has **two config directories** - a legacy one and a current one. I was editing the wrong file. (Watch the error output: it usually tells you exactly which config path it's reading.) The error message actually told me which file it was reading, but I didn't notice until the third attempt.

**Lesson one:** Read the error messages. All of them.

---

## The Setting That Does Nothing

Fixed the config file issue. Still connecting to localhost.

I stared at my config. The gateway URL was right there, pointing to my server. Why was it being ignored?

Because having the URL configured doesn't mean it's *used*. There's a separate "mode" setting that switches between local and remote. Without explicitly setting `mode: "remote"`, Moltbot ignores your remote configuration entirely.

It's like putting an address in your GPS but never hitting "Start Navigation."

**Lesson two:** Configuration presence doesn't mean configuration activation.

This is a mode toggle problem, not a URL problem.

---

## The Port That Wasn't Listening

Before you dig deeper into app configuration, confirm the **gateway is reachable over the network**. Most failures here are basic connectivity issues:

- **Gateway not running** (or crashed) on the server
- **Gateway bound to localhost** instead of your LAN interface
- **Firewall rules** blocking inbound traffic to the gateway port
- **Wrong IP/port** in the client configuration

What to do:

1. **Verify the gateway service is up** on the server and listening on the expected port.
2. **Allow the gateway port from your LAN only** (don't expose it broadly to the internet).
   - Side note: if you ever port-forward this, you're turning a private assistant into a public service. Don't.
3. **Validate end-to-end connectivity from the Windows client** to the server's gateway address before troubleshooting anything else.

Once you've established basic connectivity, the remaining issues tend to be configuration and pairing - not networking.

## The Bouncer at the Door

Gateway running. Config correct. Mode set. Surely now?

```text
Error: pairing required
```

Moltbot gateways don't let just anyone connect. There's a pairing system - devices request access, the server approves them.

Under the hood: pairing approval is a server-side control plane step, not something the client can self-complete. Security feature. Totally fair.

The frustrating part: the error tells you pairing is required but not *where* to do it. I spent ten minutes looking for a pairing command on my Windows client.

It's on the server. You approve devices from the gateway side, not the client side.

```bash
moltbot devices list              # see pending requests
moltbot devices approve <id>      # let them in
```

**Lesson four:** When an error doesn't tell you where to fix it, try the other machine.

---

## The Error That Wasn't the Error

Throughout all this, every failure triggered a secondary message:

```text
No API key found for provider "anthropic"
```

This was maddening because the whole point (for me) was: **local model, private lane**.

Here's what was happening: when the gateway connection fails, Moltbot tries to fall back to direct API access. That fallback also fails (no API key), generating a second error.

The API key error was a symptom of gateway failure, not a problem to solve directly - basically a downstream domino after the gateway path breaks. Fix the gateway, and it disappears.

**Lesson five:** Not every error message is the root cause. Some are just dominoes falling.

---

## The Moment It Worked

After fixing all five issues - right config file, mode setting, gateway service, device pairing, ignoring the red herring - I ran the command again.

Response from my local model. On my Windows machine. Processed by my Ubuntu server. Over my local network.

This is the moment that makes local AI "click": it stops being a demo and becomes a *private capability* you can actually build around.

---

## What I'd Tell Past Me

If I were starting over, here's the checklist I'd follow:

### On the server
1. Get Ollama running with a model
2. Install Moltbot and configure the Ollama provider
   - use the `openai-completions` API, not `responses`
3. Set gateway to bind to **LAN**, not just localhost
4. Start the gateway service (and verify it stays up)
5. Open the firewall for your **local subnet only**

### On the client
1. Install Moltbot
2. Set `gateway.mode` to `remote` (not just the URL!)
3. Point to your server's gateway URL
4. Run `moltbot doctor --fix` to initialize auth
5. Test network connectivity before anything else
6. Approve the device from the server side

### When things break
- Check which config file is actually being read (it's in the error output)
- Verify the port is reachable before debugging config
- Device pairing happens server-side
- Ignore "no API key" errors until the gateway works

---

## Was It Worth the Hour?

Yes - because it unlocked something specific: a reliable "private lane" for AI.

I still use cloud agents where they make sense. Local is just the lane I reserve for the stuff I don't want leaving home. But now I have a setup where I can confidently route personal workflows to a local model without wondering where that data ends up.

The setup friction was real - five distinct problems across two machines - but they're all solvable, and once solved, they tend to stay solved.

If you're considering running local AI: yes, it works. Yes, you'll hit weird config issues. And yes, you can debug it in an afternoon - as long as you don't let config files gaslight you.

---

*Technical details: Moltbot 2026.1.27-beta, Ollama 0.5.x, Qwen 30B model, Ubuntu server, Windows client. Full configuration snippets below.*

---

## Appendix: Configuration Reference

<details>
<summary>Server config (~/.moltbot/moltbot.json)</summary>

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "ollama": {
        "baseUrl": "http://127.0.0.1:11434/v1",
        "apiKey": "ollama",
        "api": "openai-completions",
        "models": [
          {
            "id": "mychen76/qwen3_cline_roocode:30b",
            "name": "Qwen3 Cline Roocode 30B",
            "reasoning": false,
            "input": ["text"],
            "cost": { "input": 0, "output": 0, "cacheRead": 0, "cacheWrite": 0 },
            "contextWindow": 120000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "gateway": {
    "mode": "local",
    "bind": "lan",
    "auth": { "token": "YOUR_TOKEN" },
    "remote": { "token": "YOUR_TOKEN" }
  },
  "agents": {
    "defaults": {
      "model": { "primary": "ollama/mychen76/qwen3_cline_roocode:30b" }
    }
  }
}
```

</details>

<details>
<summary>Client config (~/.moltbot/moltbot.json)</summary>

```json
{
  "gateway": {
    "mode": "remote",
    "auth": { "mode": "token", "token": "LOCAL_AUTH_TOKEN" },
    "remote": {
      "url": "ws://YOUR_SERVER_IP:18789",
      "token": "YOUR_GATEWAY_TOKEN"
    }
  }
}
```

</details>

<details>
<summary>Useful commands</summary>

```bash
# Generate a secure token
openssl rand -hex 32

# Check if gateway port is reachable (Windows)
Test-NetConnection -ComputerName SERVER_IP -Port 18789

# Check if gateway port is reachable (Linux/Mac)
nc -zv SERVER_IP 18789

# Start gateway service
systemctl --user start moltbot-gateway.service

# Approve a device (on server)
moltbot devices list
moltbot devices approve <device-id>

# Test the connection
moltbot agent --agent main -m "Hello"
```

</details>

<details>
<summary>Common errors quick reference</summary>

| What you see | What it means | What to do |
|--------------|---------------|------------|
| `Bind: loopback` | Mode not set to remote / bind not LAN | Add remote mode + bind LAN |
| `TcpTestSucceeded: False` | Gateway not reachable | Check service is running, check firewall |
| `pairing required` | Device not approved | Run `moltbot devices approve` on server |
| `No API key found` | Gateway failed | Fix the gateway connection first |
| `Unknown model` / HTTP 404 | Wrong Ollama API type | Use `"api": "openai-completions"` |

</details>
