# Investigation 1.1: Who Is My Computer Talking To?

**Difficulty:** Beginner
**Time budget:** 45 to 60 minutes
**Machine:** Your own Windows 11 laptop

## The scenario

Module 00 taught you to baseline what's running. Now your manager asks a harder question:

> "Fine, you know what's running. But is any of it talking to the outside world right now, and to whom? A justified process can still be phoning somewhere it shouldn't."

Same discipline as before: observe, don't assume, back every verdict with evidence.

## Your mission

1. List every active network connection on your machine right now.
2. For each one you can identify, find out: which process owns it, what remote address/port it's connected to, and what state the connection is in.
3. Classify a handful of the more interesting ones into JUSTIFIED / UNKNOWN / SUSPICIOUS - same three buckets as Module 00, now applied to connections instead of processes.
4. Write your findings using the Observation → Evidence → Reasoning → Conclusion → Confidence format (PHILOSOPHY.md rule 11) for at least 3 connections.

## Rules of engagement

- Evidence only. "That's probably fine" is not an answer.
- Document every command you run.
- Getting stuck is expected. Write down where.

## Starter commands

```powershell
# All active TCP connections, with the owning process ID
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, OwningProcess | Sort-Object State

# Turn an OwningProcess PID into an actual process name (you know this move from Module 00)
Get-Process -Id <PID>

# Save your evidence
Get-NetTCPConnection | Select-Object LocalAddress, LocalPort, RemoteAddress, RemotePort, State, OwningProcess | Export-Csv "..\..\labs\01-networking\evidence\netconn-snapshot-$(Get-Date -Format 'yyyy-MM-dd-HHmm').csv" -NoTypeInformation
```

Questions you'll likely run into (don't look these up until you hit them):

- What does `State: Listening` mean versus `State: Established`? Are both worth the same level of attention?
- A `RemoteAddress` of `0.0.0.0` or your own local IP - what does that mean?
- One process (OwningProcess) can own multiple connections at once - which process from Module 00 would you expect to see with the most connections, and why?
- Some connections will resolve to unfamiliar remote IPs. How would you start figuring out what they are, without assuming malicious?

## Deliverables

In `labs/01-networking/evidence/`:

1. `netconn-snapshot-<date>.csv` - raw capture
2. `investigation-report.md` - same structure as Module 00's report, but each finding written in Observation → Evidence → Reasoning → Conclusion → Confidence form

## When you're done

Bring the report back. We'll review it, build your stuck-list into `lesson.md` (Layer 1 only, same lesson as promised: right-sized for where you actually are), and close the module the same way we closed Module 00.
