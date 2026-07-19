# Notes: Module 00

Your working notes. Messy is fine. Write while you investigate, not after.

## Core Principle #1: Evidence First

> A program is a set of instructions. A process is a running instance of that program. Whether a file is malicious and whether it is currently executing are separate questions. I should only make claims that my evidence supports.

This is the first cybersecurity principle earned from Investigation 0.1. It will come back in networking, logs, malware analysis, and incident response - every one of those is ultimately the same discipline: separate what the evidence proves from what I'm assuming.

## Prediction: What is a process? (before the lesson)

- **My guess:** A process was simply a current running activity in Windows.
- **How I think it works:** One program would normally have one process; didn't know why there were so many Chrome/svchost.exe entries.
- **Confidence:** 40%
- **Why I think that:** Never used PowerShell before. After running `Get-Process`, saw many processes but did not understand what they represented or why there were so many.

## Prediction vs. reality (after the lesson)

- **What I got right:** A process is tied to something running, not something static.
- **What I got wrong:** Assumed one program = one process. Didn't understand why Chrome or svchost.exe would multiply.
- **What I learned:** A program is a set of instructions stored on disk. A process is a running instance of that program after Windows loads it into memory and executes it. One program can create many processes (Chrome isolates tasks; svchost.exe hosts many different Windows services under one shared file). A process name alone does not prove legitimacy - analysts rely on file path, parent process, and behavior instead.
- **Confidence after:** 90% - can now explain program vs. process using Chrome/svchost.exe/a hypothetical malicious example, and understand why analysts must investigate evidence instead of trusting filenames.

## Quiz answers (from own understanding, no re-reading lesson.md)

1. **Program vs. process:** A program is a set of instructions stored on the computer. A process is the running instance of that program after Windows loads it into memory and starts executing it.
2. **Why Chrome has many processes:** Chrome separates different tasks into different running processes, so if one part crashes, the others can keep working.
3. **Why many svchost.exe is normal:** Windows starts many separate instances of svchost.exe to host different background services. Same program file, but each running instance is its own process with its own PID.
4. **Can you trust chrome.exe/svchost.exe by name alone?** No - malware can reuse a legitimate name. Must also check file path, parent process, and behavior.
5. **chrome.exe running from C:\Users\Taiwo\Downloads - first response?** Classify as suspicious and keep investigating, not an instant malware verdict. The location is unusual for Chrome, but more evidence is needed (where it came from, who started it, what it's doing) before concluding malicious.

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

1. What is a process, really? - **ANSWERED**
2. Why are there so many processes? - **ANSWERED**
3. Who starts each process? - partially answered (parent process concept); full depth deferred to Module 01-02
4. Which processes belong to Windows vs. installed software? - answered via path/parent evidence
5. Can malware disguise itself as a normal process (e.g. fake svchost.exe)? - **ANSWERED**
6. How can I determine whether a process is safe? - answered at Layer 1 (path, parent, behavior); code signing deferred
7. Why are there multiple instances of the same process name (svchost.exe, chrome.exe)? - **ANSWERED**
8. Why does every process have a PID? - touched on; not yet deeply explained
9. How do processes communicate with each other? - not yet covered, carried forward
10. Why did most CommandLine values come back blank, and does running PowerShell as Administrator change that? - not yet confirmed, carried forward
11. What does a process's ParentProcessId actually tell an investigator? - Layer 1 answer given; full depth deferred to Module 01-02

## Struggle log (honest)

Wrote raw PowerShell commands before knowing basic syntax (the `|` pipe pattern). Found synthesizing an answer from CommandLine output harder than running the command itself - reading Chrome's `--type=renderer` flags and reasoning "why would a browser do this" was the hardest part, not the typing. This is normal productive confusion, not a sign the method is wrong.
