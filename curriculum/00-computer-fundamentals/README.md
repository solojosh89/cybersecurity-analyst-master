# Module 00: Computer Fundamentals

**Status:** COMPLETE
**Started:** 2026-07-19
**Completed:** 2026-07-19

You do not start this module by reading. You start it by investigating your own machine. Theory comes after you have floundered, because that is when it sticks.

## What was learned

- The difference between a program (inert, on disk) and a process (running, in memory, with a PID)
- One program can spawn many processes (svchost.exe's `-k`/`-s` service groups; Chrome's `--type=` isolation)
- A process name alone proves nothing - legitimacy is established through path, parent process, and behavior
- Console output (`Format-Table -AutoSize`) can silently truncate evidence; raw export (`Export-Csv`) doesn't
- Core Principle #1: Evidence First

## Key commands

`Get-CimInstance Win32_Process`, `Group-Object`, `Sort-Object`, `Select-Object`, `Export-Csv`, `Get-AuthenticodeSignature` (introduced, not yet used)

## Common mistakes (mine, honestly)

- Assumed one program = one process
- Jumped toward conclusions before gathering enough evidence to support them
- Trusted a process name at first glance before learning to check path/parent instead

## Artifact checklist

- [x] Investigation ([investigation.md](./investigation.md), report in [evidence/investigation-report.md](../../labs/00-computer-fundamentals/evidence/investigation-report.md))
- [x] Lesson ([lesson.md](./lesson.md))
- [x] Lab (fulfilled by Investigation 0.1 - see [lab.md](./lab.md))
- [x] Notes ([notes.md](./notes.md))
- [x] Quiz ([quiz.md](./quiz.md), answered in notes.md)
- [x] Portfolio Evidence ([portfolio.md](./portfolio.md))
- [x] Reflection ([reflection.md](./reflection.md))
- [x] Git commit that represents genuine learning

## Exit gate

> Can you prove that you know this?

**Yes.** Evidence: [investigation-report.md](../../labs/00-computer-fundamentals/evidence/investigation-report.md) (281 processes classified with path/parent evidence), [portfolio.md](./portfolio.md) (employer-facing summary), [reflection.md](./reflection.md) (independent quiz answers scoring correctly on the "suspicious path" scenario without re-reading the lesson).

## Next

Module 01 - Networking
