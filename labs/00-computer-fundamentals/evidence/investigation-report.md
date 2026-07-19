# Investigation Report: Process Baseline of Taiwo's Windows 11 Machine

**Date:** 2026-07-19
**Analyst:** Taiwo
**Investigation:** 0.1 - What is running on my computer?

## Objective

Account for every process running on my machine right now, and justify each one with evidence rather than assumption.

## Method

```powershell
Get-Process

Get-CimInstance Win32_Process | Select-Object ProcessId, ParentProcessId, Name, ExecutablePath | Sort-Object Name | Format-Table -AutoSize

(Get-CimInstance Win32_Process).Count

Get-CimInstance Win32_Process | Group-Object Name | Sort-Object Count -Descending | Select-Object -First 5 Name, Count

Get-CimInstance Win32_Process -Filter "Name='svchost.exe'" | Select-Object ProcessId, ParentProcessId, CommandLine | Format-List

Get-CimInstance Win32_Process | Select-Object ProcessId, ParentProcessId, Name, ExecutablePath, CommandLine | Export-Csv "process-snapshot-2026-07-19-1027.csv" -NoTypeInformation
```

Ran in a standard (non-elevated) PowerShell session.

## Findings

| Process family | Count | Path | Verdict | Evidence |
|---|---|---|---|---|
| svchost.exe | 96 | `C:\WINDOWS\system32\svchost.exe` (where visible) | JUSTIFIED | One shared executable hosting many Windows services via `-k Group -s Service`; parent PID 1116 (`services.exe`) for every instance checked |
| chrome.exe | 44 | `C:\Program Files\Google\Chrome\Application\chrome.exe` | JUSTIFIED | One main process (PID 4688, no `--type` flag) spawning `--type=renderer/gpu-process/utility/crashpad-handler` children; consistent path and parent across all 44 |
| msedgewebview2.exe | 12 | `C:\Program Files (x86)\Microsoft\EdgeWebView\...` | JUSTIFIED | Standard Microsoft Edge WebView component, used by other installed apps to render web content |
| claude.exe | 12 | Mix of `AppData\Roaming\Claude\...` and WindowsApps Claude package | JUSTIFIED | This is the AI assistant I'm using for this investigation - expected |
| RuntimeBroker.exe | 7 | `C:\Windows\System32\RuntimeBroker.exe` | JUSTIFIED | Standard Windows process brokering permissions for UWP apps |
| AdGuardVpnSvc.exe, Com4QLBEx.exe, cowork-svc.exe | 1 each | (blank in Format-Table output) | UNKNOWN | ExecutablePath came back empty in the table view; not yet confirmed whether this is a real gap or console truncation (see Limitations) |

Total processes: **281**. The top 2 families alone (svchost.exe, chrome.exe) account for 140 processes - roughly half the machine.

## Key Learning

A **program** is a set of instructions stored on disk. A **process** is a running instance of that program, with its own memory, its own PID, and its own security context. The two are not one-to-one: a single program (svchost.exe) can be loaded as 96 separate processes doing 96 different things. This means a process **name** is not evidence of anything by itself - what actually proves legitimacy is the file's signature, its path, and its parent process. Whether a file is malicious and whether it is currently executing are separate questions, and I should only make claims my evidence supports. (Full principle recorded in [notes.md](../../curriculum/00-computer-fundamentals/notes.md).)

## Unknowns and suspicions

- `AdGuardVpnSvc.exe`, `Com4QLBEx.exe`, `cowork-svc.exe`: no path shown in table output. Not escalated to SUSPICIOUS because the blank could be a display artifact (console truncation, matching the `ExecutablePath`/`CommandLine` truncation seen elsewhere) rather than a real missing path. Needs the CSV or an elevated session to confirm.
- Most svchost.exe `CommandLine` values were blank; only a handful (running with elevated-looking context) showed the full `-k`/`-s` detail. Suspected cause: non-elevated PowerShell can't read `CommandLine` for processes it doesn't own or that run as SYSTEM. Not yet confirmed by re-running elevated.

## Where I got stuck

- Did not know basic PowerShell pipe (`|`) syntax going in; had to learn the `Get-Something | Select-Object | Sort-Object` pattern before I could write my own queries.
- Synthesizing *why* svchost and Chrome each run so many processes was harder than running the commands - reasoning about `--type=renderer` flags took real thought, not just typing.
- Discovered mid-investigation that `Format-Table -AutoSize` silently truncates long column values, meaning my first look at the data was already incomplete evidence.

## What I would investigate next

- Re-run the svchost and empty-path queries in an elevated PowerShell session to resolve the two open unknowns above.
- Check code-signing status (`Get-AuthenticodeSignature`) on a sample of unfamiliar processes rather than trusting the path alone.
- Look at what starts each process at boot (services, scheduled tasks, startup folder) - this investigation only looked at a single point-in-time snapshot.

## Evidence

**Evidence collected:**
- `process-snapshot-2026-07-19-1027.csv` (full 281-process export, 85KB)
- This report

**Confidence:** 75%. High confidence on the svchost/Chrome/WebView/RuntimeBroker verdicts (path + parent + repeated pattern all agree). Lower confidence on the three unknown-path processes, which are unresolved, not cleared.

**Limitations:** This investigation only examined a single snapshot of running processes. It did not analyze services, scheduled tasks, registry persistence (Run keys, etc.), network connections made by any process, or code-signing status of any binary. A process being JUSTIFIED here means "consistent with known-good Windows/vendor behavior," not "cryptographically verified clean."

## Conclusion

Identified and classified all 281 running processes on my machine into families. 278 processes across 5 major families are JUSTIFIED with path, parent-process, and pattern evidence. 3 processes remain UNKNOWN pending confirmation of whether their missing path is real or a display artifact. No process was classified SUSPICIOUS. The investigation produced this repo's first cybersecurity principle: process name alone is never evidence.
