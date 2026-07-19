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

## Deferred on purpose (coming later, not forgotten)

- **Module 01-02:** virtual memory, threads, security tokens, ParentProcessId in depth, Service Control Manager (`services.exe`), Chrome's full multi-process architecture, process masquerading, code signing (`Get-AuthenticodeSignature`)
- **Later, advanced:** Windows internals, sandbox isolation, EDR detection logic, process injection, PPID spoofing, kernel object model

Nothing here was wrong, it was just too much for lesson one. If a term from an earlier draft is still rattling around in your head, that's fine, it'll get properly built on when its module comes up.
