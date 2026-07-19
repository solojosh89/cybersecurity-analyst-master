# Investigation 0.1: Who Is Running On This Machine?

**Difficulty:** Beginner
**Time budget:** 60 to 90 minutes
**Machine:** Your own Windows 11 laptop

## The scenario

You are a brand new SOC analyst. Your manager walks over and says:

> "Before you can spot an intruder's process, you need to know what normal looks like. Here is your first task: account for every process running on your workstation right now. Every single one. If you cannot explain why a process exists, flag it. Bring me evidence, not opinions."

That is the whole job in miniature: baseline, investigate, justify, report.

## Your mission

1. Capture a snapshot of every running process on this machine.
2. For each process, answer: what is it, who started it, why is it running?
3. Classify every process into one of three buckets:
   - **JUSTIFIED** - you know what it is and why it is here, with evidence
   - **UNKNOWN** - you could not confidently identify it
   - **SUSPICIOUS** - something about it does not add up (unsigned, weird path, weird parent)
4. Write an investigation report.

## Rules of engagement

- **Evidence only.** "It's probably Windows stuff" is not an answer. Show the binary path, the signer, the parent process.
- **Do not kill anything.** This is observation, not response. (Also a house rule: we never kill processes by name, only by PID, and only when we mean to.)
- **Document every command you run.** Your report should let someone else reproduce your work exactly.
- **It is fine to be stuck.** Write down WHERE you got stuck and what you tried. That list becomes the lesson.

## Starter ammunition (deliberately incomplete)

You get three commands to start. Everything else you figure out or bring to your mentors as questions.

```powershell
# Snapshot: every process with its PID and parent PID
Get-CimInstance Win32_Process | Select-Object ProcessId, ParentProcessId, Name, ExecutablePath, CommandLine | Sort-Object Name

# Who publishes this binary? (signature check on one path)
Get-AuthenticodeSignature "C:\Windows\System32\svchost.exe" | Select-Object Status, SignerCertificate

# Save your snapshot as evidence
Get-CimInstance Win32_Process | Select-Object ProcessId, ParentProcessId, Name, ExecutablePath, CommandLine | Export-Csv "..\..\labs\00-computer-fundamentals\evidence\process-snapshot-$(Get-Date -Format 'yyyy-MM-dd-HHmm').csv" -NoTypeInformation
```

Questions you will run into (do not look these up until you hit them):

- Why are there so many processes called `svchost.exe`? Are they all legitimate?
- What does the parent PID actually tell you? What if a parent PID points to a process that no longer exists?
- Which processes have NO executable path? Why?
- What is the difference between a process signed by Microsoft and one that is not signed at all?

## Deliverables

Put these in `labs/00-computer-fundamentals/evidence/`:

1. `process-snapshot-<date>.csv` - your raw capture
2. `investigation-report.md` - your report, using the structure below

### Report structure

```markdown
# Investigation Report: Process Baseline of <hostname>
Date:
Analyst: Taiwo

## Summary
(3 to 5 sentences: what you did, what you found, headline numbers)

## Method
(every command you ran, in order, and why)

## Findings
| Process | PID | Parent | Path | Signer | Verdict | Evidence |
(one row per process or per family, e.g. all Chrome processes as one family)

## Unknowns and suspicions
(everything you could not justify, and what you tried)

## Where I got stuck
(honest list - this becomes the lesson plan)

## What I would investigate next
```

## When you are done

Bring the report back here. I will review it the way a SOC lead reviews a junior analyst's first case: the verdicts you got right, the evidence that is weak, and the questions you did not think to ask. Then we write [lesson.md](./lesson.md) together from your stuck-list, and you take the quiz.
