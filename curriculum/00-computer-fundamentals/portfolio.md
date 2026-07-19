# Portfolio Evidence: Module 00 - Computer Fundamentals

## Objective

Establish a process baseline on a live Windows 11 workstation: identify every running process and justify each with evidence rather than assumption.

## Investigation

Full report: [Investigation 0.1 - Process Baseline](../../labs/00-computer-fundamentals/evidence/investigation-report.md)

Method: PowerShell (`Get-CimInstance Win32_Process`), grouped and cross-referenced by name, PID, parent PID, path, and command line.

## Evidence

- [process-snapshot-2026-07-19-1027.csv](../../labs/00-computer-fundamentals/evidence/process-snapshot-2026-07-19-1027.csv) - full 281-process export
- [investigation-report.md](../../labs/00-computer-fundamentals/evidence/investigation-report.md) - findings, verdicts, confidence score, stated limitations

## Conclusion

281 processes classified into families using path and parent-process evidence: 278 JUSTIFIED (svchost.exe, chrome.exe, msedgewebview2.exe, claude.exe, RuntimeBroker.exe), 3 UNKNOWN pending an elevated-session recheck, 0 SUSPICIOUS. Confidence: 75%, with explicitly stated limitations (single point-in-time snapshot; no services, scheduled tasks, or network activity examined).

## Skill demonstrated

> "I can baseline a Windows host and justify every running process with evidence, rather than assuming a familiar-looking name is safe."

## Core principle produced by this work

A process name alone is never evidence. Legitimacy is established through path, parent process, and behavior.
