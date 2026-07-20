# Lesson 1: Why Networks Exist

## Theory

A single computer, disconnected from everything else, can only work with what's already stored on it. The moment you want two computers to share anything, a file, a message, a webpage, they need an agreed way to send information back and forth. That agreed way is a **network**.

Every network, from two laptops connected by a cable to the entire internet, is solving the same basic problem: **how do multiple independent devices exchange information reliably, when they don't share memory and might not even trust each other?**

That framing matters more than it looks. It's the same question a cybersecurity analyst asks about every connection they investigate: what are these two devices sharing, was it supposed to happen, and can I trust what's being exchanged?

## See it for yourself (2 minutes, no theory required)

Open PowerShell and run:

```powershell
Get-NetAdapter | Select-Object Name, InterfaceDescription, Status
```

This lists the network adapters your machine actually has, e.g. Wi-Fi, Ethernet, maybe a VPN adapter. You don't need to interpret this deeply yet, just notice: your computer already has one or more physical or virtual "doors" it uses to talk to other devices. That's the starting point for everything in this module.

## SOC Connection

Every single thing a SOC analyst investigates, an intrusion, a data leak, malware calling home, is fundamentally a network doing exactly what networks are designed to do: moving information between devices. The job of a defender isn't to prevent networking, it's to tell legitimate information exchange apart from illegitimate information exchange. You can't do that until you understand what "normal" exchange looks like, which is why this module builds from the ground up.

## Worked example

**Observation:** Your `Get-NetAdapter` output shows a "Wi-Fi" adapter with Status: Up, and possibly a "VPN" or "Ethernet" adapter with Status: Disconnected.

**Evidence:** The Status column directly reports whether that adapter currently has an active link.

**Reasoning:** An adapter that's "Up" is one your machine could currently be sending or receiving data through. An adapter that's "Disconnected" exists as hardware/software but isn't currently in use.

**Conclusion:** Right now, your machine's active network path is whichever adapter shows "Up" - that's the one worth paying attention to first when something needs investigating.

**Confidence:** High - this is directly reported by Windows, not inferred.

## Classwork

1. From your `Get-NetAdapter` output, list every adapter you have and its status. For each one, write one sentence guessing what it's for (Wi-Fi, Ethernet, VPN, virtual/loopback adapters from software like Docker or a VPN client all show up here).
2. In your own words, one or two sentences: why can't two computers just share memory directly instead of needing a network?

Answers go in `notes.md` under Lesson 1.
