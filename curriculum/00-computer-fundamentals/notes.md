# Notes: Module 00

Your working notes. Messy is fine. Write while you investigate, not after.

## Core Principle #1: Evidence First

> A program is a set of instructions. A process is a running instance of that program. Whether a file is malicious and whether it is currently executing are separate questions. I should only make claims that my evidence supports.

This is the first cybersecurity principle earned from Investigation 0.1. It will come back in networking, logs, malware analysis, and incident response - every one of those is ultimately the same discipline: separate what the evidence proves from what I'm assuming.

## Prediction: What is a process? (before the lesson)

- My guess:
- How I think it works:
- Confidence (low/medium/high):
- Why I think that:

## Prediction vs. reality (after the lesson)

- What I got right:
- What I got wrong:
- What surprised me:

## Commands that worked

```powershell
Get-Process

Get-CimInstance Win32_Process | Select-Object ProcessId, ParentProcessId, Name, ExecutablePath | Sort-Object Name | Format-Table -AutoSize

(Get-CimInstance Win32_Process).Count

Get-CimInstance Win32_Process | Group-Object Name | Sort-Object Count -Descending | Select-Object -First 5 Name, Count

Get-CimInstance Win32_Process -Filter "Name='svchost.exe'" | Select-Object ProcessId, ParentProcessId, CommandLine | Format-List
```

## Findings

- Total processes on this machine: **281**
- Top process counts: svchost.exe = 96, chrome.exe = 44, msedgewebview2.exe = 12, claude.exe = 12, RuntimeBroker.exe = 7
- svchost.exe and chrome.exe together account for roughly half of every process running on the machine
- Most svchost.exe `CommandLine` values came back blank in a non-elevated PowerShell session; only a handful showed full detail (`-k GroupName -s ServiceName`) - suspected permissions issue, to be confirmed by re-running elevated
- Chrome runs 44 processes: one main process (no `--type` flag) plus many `--type=renderer` (looks like one per tab/site), `--type=gpu-process`, `--type=utility`, `--type=crashpad-handler`
- `claude.exe` and `powershell.exe` show up in the process list too - the investigation tool is itself part of the system being investigated

## Things I did not expect

- One executable name (`svchost.exe`) can represent 96 completely different running services, not 96 copies of the same thing
- `Format-Table -AutoSize` silently truncates long values (ExecutablePath, CommandLine) - raw console output is not reliable evidence; `Export-Csv` preserves full strings
- A browser splits itself into dozens of separate OS processes on purpose, for crash isolation and security, rather than running as one process

## Questions for my mentors

1. What is a process, really? (asked before knowing the `-k`/`-s` answer for svchost)
2. Why are there so many processes?
3. Who starts each process?
4. Which processes belong to Windows vs. installed software?
5. Can malware disguise itself as a normal process (e.g. fake svchost.exe)?
6. How can I determine whether a process is safe?
7. Why are there multiple instances of the same process name (svchost.exe, chrome.exe)? (partially answered - see Findings)
8. Why does every process have a PID?
9. How do processes communicate with each other?
10. Why did most CommandLine values come back blank, and does running PowerShell as Administrator change that?
11. What does a process's ParentProcessId actually tell an investigator?

## Struggle log (honest)

Wrote raw PowerShell commands before knowing basic syntax (the `|` pipe pattern). Found synthesizing an answer from CommandLine output harder than running the command itself - reading Chrome's `--type=renderer` flags and reasoning "why would a browser do this" was the hardest part, not the typing. This is normal productive confusion, not a sign the method is wrong.
