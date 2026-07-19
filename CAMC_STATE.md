# CAMC State

> The single source of truth for where this curriculum stands. Any AI mentor or future session starts here. Updated at the end of every study session.

**Last updated:** 2026-07-19

## Current position

- **Phase:** 1 (foundation, target completion December 2026)
- **Module:** 00 - Computer Fundamentals
- **Current investigation:** Investigation 0.1 - What is running on my computer?
- **Status:** IN PROGRESS - evidence gathered, synthesis and formal lesson not yet written

## Module 00 artifact status

- [ ] Investigation (IN PROGRESS - process snapshot taken, findings logged in notes.md, formal report not yet written to `labs/00-computer-fundamentals/evidence/`)
- [ ] Lesson (next up: "What is a Process?" - see Next Action)
- [ ] Lab
- [x] Notes (in progress, being updated live - see [notes.md](curriculum/00-computer-fundamentals/notes.md))
- [ ] Quiz (LOCKED until lesson exists)
- [ ] Portfolio Evidence
- [ ] Reflection

## What's been learned so far (evidence-backed)

- PowerShell basics: the `Get-Something | Select-Object | Sort-Object` pipe pattern
- Ran `Get-Process` and `Get-CimInstance Win32_Process` against own machine
- Total process count: 281
- Top repeaters: svchost.exe (96), chrome.exe (44), msedgewebview2.exe (12), claude.exe (12), RuntimeBroker.exe (7)
- svchost.exe hosts many different Windows services behind one shared executable (`-k Group -s Service`); most CommandLine values were blank in non-elevated PowerShell, suspected permissions gap, not yet confirmed with an elevated session
- Chrome runs one main process plus many `--type=renderer/gpu-process/utility/crashpad-handler` children, for crash isolation and security separation
- Full stuck-list of 11 questions captured in notes.md, ready to drive lesson.md

## Open questions carried between sessions

See "Questions for my mentors" in [notes.md](curriculum/00-computer-fundamentals/notes.md) - 11 questions, including: what is a process, why can malware disguise as a normal process name, what does ParentProcessId actually prove, does elevated PowerShell reveal the blank CommandLine fields.

## Next actions

1. Taiwo: answer "What is a process?" in your own words first (no lookup)
2. Mentor: teach the formal concept, compare against Taiwo's own answer
3. Optional: re-run svchost CommandLine query in an elevated PowerShell window to test the permissions theory
4. Pick a verdict (JUSTIFIED/UNKNOWN/SUSPICIOUS) for the empty-ExecutablePath process spotted earlier (AdGuardVpnSvc.exe / Com4QLBEx.exe / cowork-svc.exe)
5. Write investigation-report.md and export the CSV to `labs/00-computer-fundamentals/evidence/`
6. Co-write lesson.md from the full stuck-list, then quiz.md, then reflection.md
7. Meaningful commit closing Module 00

## Division of labor

- **ChatGPT:** first-principles explanations, lesson/quiz design, reasoning review
- **Claude Code:** local machine work, lab setup, artifact review against real evidence, commits
- **Taiwo:** the actual investigating - no artifact, no progress

## Standing constraint

Taiwo is on a Pro plan and cannot afford upgrades. Design everything model-agnostic; this state file exists specifically so any mentor (ChatGPT free tier, Claude at any tier) can resume instantly. Prefer free tools/resources (own machine as lab, Sysmon, Wireshark, TryHackMe/LetsDefend free tiers) over paid recommendations.

## Session log

| Date | What happened | Commit |
|------|---------------|--------|
| 2026-07-19 | Framework agreed; Module 00 launched; Investigation 0.1 issued | ca43413 |
| 2026-07-19 | Philosophy + state file established | efd2ad3 |
| 2026-07-19 | Investigation 0.1 evidence gathering: process counts, svchost/Chrome multi-process findings, stuck-list captured | (pending commit) |
