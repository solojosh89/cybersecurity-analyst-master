# Lesson 00: Processes, and Why "Normal" Matters

Built from Investigation 0.1's actual stuck-list. Layer 1 only, everything else deferred on purpose (see bottom).

## Why does this exist?

Windows needs to run many programs at once without them stepping on each other. The process is how it keeps them apart.

## How does it work

- A **program** is inert: a file on disk. `chrome.exe` sitting in `Program Files` isn't doing anything by itself.
- A **process** is what exists once Windows loads that program into memory and starts running it. It gets its own memory, and a unique ID: the **PID**.
- **Program and process are not one-to-one.** You proved this yourself: `svchost.exe` is one file, but you found it running as 96 separate processes. Windows launches it repeatedly, each launch is its own isolated process hosting a different Windows background service. Same file, 96 separate running copies.
- Chrome does something similar: one main process, plus many child processes (you saw the `--type=renderer`, `--type=gpu-process` flags). That's why one browser shows up as 44 rows instead of 1: each tab/feature gets isolated, so if one crashes, the rest keep running.

## How do attackers abuse it?

Windows only checks that a process *claims* a name like `svchost.exe`. It doesn't verify anything by name alone. So malware often names itself something ordinary, hoping to blend into a long list of legitimate-looking processes exactly like the 96 real ones you found.

## How do defenders detect it?

Not by name. By asking:

- **Where does it live?** Real svchost.exe lives in `C:\Windows\System32`. A copy in `Downloads` is not normal.
- **Who started it?** (This is the ParentProcessId column you already collected - what it fully means is next module's topic, for now just know: legitimate processes are usually launched by predictable parents, and an unexpected parent is a flag.)
- **What is it actually doing?** Behavior over label.

## If you only remember three things

1. A program is a set of instructions. A process is a running instance of a program.
2. A process name alone does not prove legitimacy.
3. Analysts investigate using evidence (path, parent, behavior), not assumptions.

## Worked example: applying the framework to a name you didn't find

You found real `svchost.exe` and `chrome.exe` entries on your own machine, both legitimate. Here's the same reasoning applied to a case designed to fail the "name alone" test, using the Observation → Evidence → Reasoning → Conclusion → Confidence shape from [PHILOSOPHY.md](../../PHILOSOPHY.md) rule 11.

> **Observation:** A process named `svchost.exe` is running, PID 8842.
> **Evidence:** `Get-CimInstance Win32_Process -Filter "ProcessId=8842"` shows `ExecutablePath = C:\Users\Public\svchost.exe` and `ParentProcessId` pointing to `powershell.exe`, not `services.exe`.
> **Reasoning:** Real `svchost.exe` runs from `C:\Windows\System32` and is always started by the Service Control Manager (`services.exe`) — you didn't verify that parent chain in depth yet (deferred to Module 01-02), but you already know from this lesson that `services.exe` is the expected launcher, not a shell. Both the path and the parent are wrong for what this name claims to be.
> **Conclusion:** This is not the legitimate Windows service host. It is a process using a trusted name to blend in.
> **Confidence:** High — two independent pieces of evidence (path, parent) disagree with the claimed identity, and neither is explained by anything you learned this module.

Notice what didn't happen: no scan, no antivirus verdict, no guess. Two `Get-CimInstance` columns you already collected during Investigation 0.1 were enough to build a checkable case.

## Classwork (do before Module 01's lesson lands)

These are small, use only what this lesson covered, and produce evidence you can glance back at later. Not a re-run of Investigation 0.1 — just enough reps to make path/parent-checking a reflex before networking piles on new material.

1. Pick any 3 processes from your Investigation 0.1 export that you did **not** already write up in `notes.md`. For each, write one Observation → Evidence → Reasoning → Conclusion → Confidence block like the one above, using their real path and parent from your own data.
2. Run `Get-CimInstance Win32_Process -Filter "Name='svchost.exe'"` again, elevated this time (Run PowerShell as Administrator). Compare the `CommandLine` output to what you got unelevated in Investigation 0.1. Note in `notes.md` under "Questions for my mentors" whether question 10 is now answered.
3. One sentence, no lookup: if a process's path and parent both look normal but the CPU usage is unusually high, is that enough alone to call it suspicious? Why or why not, given what "evidence-based" means in this lesson.

Drop the answers into `notes.md` under a new `## Classwork` heading. This isn't a gate on Investigation 1.1 (that's already issued and independent) — it's reinforcement so Layer 1 is solid before Layer 2 shows up.

## Deferred on purpose (coming later, not forgotten)

- **Module 01-02:** virtual memory, threads, security tokens, ParentProcessId in depth, Service Control Manager (`services.exe`), Chrome's full multi-process architecture, process masquerading, code signing (`Get-AuthenticodeSignature`)
- **Later, advanced:** Windows internals, sandbox isolation, EDR detection logic, process injection, PPID spoofing, kernel object model

Nothing here was wrong, it was just too much for lesson one. If a term from an earlier draft is still rattling around in your head, that's fine, it'll get properly built on when its module comes up.
