---
title: "Attention, Reclaimed -- AI Decluttering Agent"
date: 2026-05-16
categories:
  - AI
  - Productivity
tags:
  - email
  - automation
  - outlook
  - local-ai
  - workflow
excerpt: "I've been deliberately handing the clutter of my life off to AI -- starting with the inbox. Here's what it actually looks like when an agent does the decluttering for you."
classes: wide
---

If you read about the tax agent, you saw what this looks like when it works: an AI doing the tedious layer so you can show up with actual leverage. I've been finding that pattern in more places than I expected. The inbox is one of them. The sorting is relentless and mostly mechanical: read this, move that, figure out whether this notification matters. I have accounts covering different domains of my life -- personal and professional, one kept deliberately separate for higher-stakes communication, one that mostly receives alerts, and one that came with a volunteer role that generates far more mail than I anticipated. Each has its own volume, its own folder structure, its own definition of "important." The clutter isn't the emails themselves. It's the deciding what to do with them, at scale, day after day.

One of those accounts is an old address I grabbed early -- clean handle, convenient at the time. What nobody warns you about is that long-standing addresses are catnip for spam lists. The volume eventually got bad enough that I flipped the model: everything routes to junk by default, only approved senders reach the inbox. It works fine day-to-day. But it means legitimate emails regularly end up buried in junk, and someone has to dig through hundreds of messages to rescue them. That someone is now an AI agent.

The goal isn't to get AI to read my email. It's to get the clutter out of my attention entirely. Think about what's buried in a typical inbox: commitments that should be on the calendar, events that need to be synced with a spouse, reminders that should fire at the right time. Right now, most people either catch those manually or miss them. The version I'm working toward handles that automatically -- scour the inbox, pull out what's actionable, land it where it belongs. The decluttering agent is the first step in that chain.

## How This Works

A fair question at this point: isn't this just what AI email assistants already do? Read your inbox, help you draft a reply, summarize a thread? Those tools exist, and they're useful -- but they still put you in the loop. You're working faster, not working less. What I'm describing is different: the agent operates the inbox on my behalf, and I only see what made it through.

That's possible because of custom tooling that gives the AI direct access to my email -- not read-only summarization, but the ability to move messages, search across folders, act across multiple accounts, and flag specific items for my attention. It runs locally, on my machine, talking directly to Outlook. Nothing leaves the machine, no third-party service involved. The custom layer is what makes the difference between an AI that helps you manage email and one that manages it for you. If you want to run it yourself, the server is open source at [github.com/JackDDavis/outlook-desktop-mcp](https://github.com/JackDDavis/outlook-desktop-mcp).

## What the Agent Actually Does

The starting prompt is simple: "triage my inbox across all accounts." From there, the agent reads subjects, senders, and enough of each message to make a decision -- without me in the loop.

Service notifications from a billing platform: archived after confirming nothing needs action. Items that looked like junk but weren't: flagged for my review. The agent doesn't delete anything I haven't approved for deletion. Uncertain calls come to me. The rest it handles.

The clearest example of where this earns its keep is the account that catches messages from my kids' school. Every week brings a stream: daily notices, spirit week reminders, sick alerts, event signups. Individually harmless. Collectively, they train you to skim everything because most of it doesn't matter -- until the one that does. The agent learns which senders and subject patterns are action-required versus file-and-forget. That distinction is obvious to a human after a few weeks of exposure. It's also obvious to a model that can read several weeks of messages in about four seconds.

In one recent session it cleared 276 messages across all accounts, including the junk-first inbox. My real alternative wasn't careful reading -- it was selecting everything and deleting it, occasionally losing something I actually wanted. The agent did it in under fifteen minutes, flagged roughly forty items for my review, and moved the rest. It didn't save me hours of careful sorting. It replaced a blunt instrument with something that actually rescues the signal.

I'll be honest: I'm still refining this. I already have inbox rules set up for the obvious stuff -- the senders and patterns that are always the same. But rules are binary, and email isn't. Take the school account: fifty emails from one sender, going in completely different directions. A daily notice, a sick alert, a permission slip, a fundraiser. A rule can't tell those apart. The agent can, and it gets better at it with each pass. The goal is to keep narrowing what actually needs my attention until it's close to nothing.

One thing I was deliberate about: not every agent should be able to send email on your behalf. A general-purpose AI that can "helpfully" fire off a reply while you're asking it to sort newsletters is a problem waiting to happen. My general-purpose agent can read and organize. A dedicated email agent gets full access. The line matters.

> *For those following the agents work: what I'm describing here is a custom MCP server paired with a curated Agent Skill -- both of which I'll keep improving as the workflow evolves.*

## Attention, Reclaimed

When the agent handles the clutter, you get your attention back -- or at least, the part of it that was going to things that never needed you.

That's the actual payoff. Not the time saved in some abstract calculation, but the shift in what you're doing when you open your inbox. Instead of triaging, you're deciding. The mechanical layer is already done. The sick alert is at the top. The spirit week reminder is already filed. You never had to touch either one to know the difference.

The inbox was the right place to start because the pattern is universal and the volume is high. But it's just the first application of something broader: identifying the clutter in your life -- the high-volume, low-stakes, repetitive decisions -- and handing them off. Scheduling, receipts, status updates, notifications that need a response but not much thought. The question isn't whether an agent can handle this kind of work. It can, and it does. The question is which part of your life you declutter next.

I'm working through that list. The inbox was first because it was the most overdue.
