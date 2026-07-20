# Future Labs (Locked)

Labs that are real and coming, but gated behind prerequisite lessons so you're never memorizing a wall of unfamiliar terms. Each unlocks the moment its prerequisites are checked off in [README.md](./README.md).

---

## Lab: Get-NetTCPConnection (formerly Investigation 1.1)

**Requires:** IP Addresses ✓/✗ · Ports ✓/✗ · TCP ✓/✗ · Processes (done, Module 00) · PIDs (done, Module 00) · Connection States ✓/✗

**Status:** LOCKED

Original mission, unchanged, just waiting for its prerequisites:

> Your manager asks: "You know what's running. But is any of it talking to the outside world right now, and to whom? A justified process can still be phoning somewhere it shouldn't."

```powershell
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, OwningProcess | Sort-Object State
```

When unlocked: capture connections, identify the owning process for each (you already know `Get-Process -Id <PID>` from Module 00), classify a handful as JUSTIFIED / UNKNOWN / SUSPICIOUS using Observation → Evidence → Reasoning → Conclusion → Confidence.

---

## Lab: Wireshark packet capture

**Requires:** Packets, Ports, TCP, UDP, HTTP

**Status:** LOCKED

---

## Lab: DNS cache investigation

**Requires:** DNS

**Status:** LOCKED

---

## Lab: ARP table analysis

**Requires:** MAC Addresses, Devices and communication

**Status:** LOCKED

---

## Lab: Route table analysis

**Requires:** Routers, IP Addresses

**Status:** LOCKED
