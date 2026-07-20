# Changelog

One line per meaningful commit. This is the "how did we get here" story; [CAMC_STATE.md](CAMC_STATE.md) is "where are we now."

## 2026-07-19

- Initialized repository structure (README, ROADMAP, CONTRIBUTING, LICENSE).
- Launched Module 00, issued Investigation 0.1 (process baseline of own machine).
- Established PHILOSOPHY.md and CAMC_STATE.md as the cross-session operating rules.
- Ran Investigation 0.1: found 281 processes, svchost.exe (96) and chrome.exe (44) are half the machine; learned the `-k`/`-s` service-grouping pattern and Chrome's multi-process isolation; logged an 11-question stuck-list.
- Adopted final documentation architecture: CAMC_STATE.md as a dashboard only, CHANGELOG.md for history, module README.md as the per-module summary.
- Added Prediction/Prediction-vs-reality as the standing first section of every module's notes.md.
- Adopted Observation-Evidence-Reasoning-Conclusion-Confidence as the standard format for every investigation claim (PHILOSOPHY.md rule 11).
- **Completed Module 00 - Computer Fundamentals.** All 8 artifacts done: 281-process baseline investigation (278 JUSTIFIED, 3 UNKNOWN, 0 SUSPICIOUS), Layer-1-scoped lesson, quiz answered independently and correctly, portfolio summary, honest reflection. Earned Core Principle #1: a process name alone is never evidence.
- Added GLOSSARY.md as a lightweight term index (links back to lessons, no duplicated explanations).
- **Launched Module 01 - Networking.** Issued Investigation 1.1: "Who is my computer talking to?" (active TCP connections, owning process, JUSTIFIED/UNKNOWN/SUSPICIOUS verdicts).
- Adopted Worked Example + Classwork as a standing part of every lesson.md (CAMC_STATE.md standing conventions); added both retroactively to Module 00's lesson.md (a fake-svchost.exe walkthrough, 3 reinforcement tasks).
- **Restructured Module 01 to a 17-step prerequisite-gated sequence**, a documented one-module exception to investigate-before-study, decided by Taiwo after mentors disagreed on sequencing. Hands-on lab (Get-NetTCPConnection) moved to future-labs.md, locked until IP/Ports/TCP/States are covered. Lesson 1 (Why Networks Exist) written and issued.
