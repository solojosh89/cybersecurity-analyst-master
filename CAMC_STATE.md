# CAMC State

> Save-game file. Any mentor (me, ChatGPT, future AI) reads this first and knows exactly where we are. Detail lives in module files, not here - see [CHANGELOG.md](CHANGELOG.md) for history.

**Last updated:** 2026-07-19

## Where we are

- **Phase:** 1 of 2 (foundation, target December 2026)
- **Module:** [01 - Networking](curriculum/01-networking/README.md) - IN PROGRESS (theory-first, see note below)
- **Previous module:** [00 - Computer Fundamentals](curriculum/00-computer-fundamentals/README.md) - COMPLETE

## Status

Module 01 restructured 2026-07-19: 17-step prerequisite-gated lesson sequence (see [curriculum/01-networking/README.md](curriculum/01-networking/README.md)), hands-on lab moved to [future-labs.md](curriculum/01-networking/future-labs.md), locked until IP/Ports/TCP/States are covered. Lesson 1 (Why Networks Exist) written and issued.

## Current focus

Taiwo reports 18 networking topics covered on the ChatGPT side (DNS, IP, MAC, DHCP, ARP, switches, routers, packets, ports, TCP/UDP, HTTP/HTTPS, GET/POST, status codes, cookies, sessions, auth, firewalls) - claimed ~85% of Module 01. Not yet reflected as repo evidence (rule 1: repo is the referee). A checkpoint quiz ([quiz.md](curriculum/01-networking/quiz.md), 18 questions) has been issued to convert the claim into real, checkable evidence before marking anything complete.

## Current blocker

Taiwo answering the Module 01 checkpoint quiz from memory in notes.md. Once in: mark verified topics complete in README.md, identify genuinely shaky spots (those become real next lessons, not filler), and - since IP/Ports/TCP/States are among the claimed topics - likely unlock and actually run the Get-NetTCPConnection lab in future-labs.md for the first time.

## Next lesson

Pending checkpoint quiz results. NOT necessarily NAT - only once everything already claimed is verified.

## Documented exception (2026-07-19)

Module 01 deliberately does NOT follow investigate-before-study (PHILOSOPHY.md rule 2). Decision made explicitly by Taiwo after a disagreement surfaced between mentors: ChatGPT proposed a full 17-step prerequisite gate before any networking command; Claude recommended keeping investigate-first with a light primer. Taiwo chose the full gate, reasoning "I just started learning" - networking vocabulary (port, TCP, state) isn't intuitive the way "a running program" was for Module 00. This is scoped to Module 01 only; Module 02+ defaults back to investigate-first unless there's similarly good reason not to.

## Standing constraint

Taiwo is on a Pro plan, no budget for upgrades. Keep everything model-agnostic and free-resource-first (own machine as lab, Sysmon, Wireshark, TryHackMe/LetsDefend free tiers).

## Division of labor

- **ChatGPT:** first-principles explanations, lesson/quiz design, reasoning review
- **Claude Code:** local machine work, artifact review against real evidence, commits
- **Taiwo:** the investigating - no artifact, no progress

## Standing conventions

- Every module: 8 artifacts (investigation, lesson, lab, notes, quiz, portfolio, reflection, meaningful commit)
- Every lesson: teach only what's needed for THIS module; defer advanced depth explicitly rather than dumping it all at once
- Every lesson.md also includes a **Worked example** (a fresh scenario, not a repeat of the investigation, run through Observation→Evidence→Reasoning→Conclusion→Confidence) and a **Classwork** section (small reinforcement tasks, answered in notes.md, done before the next module's lesson lands - not a gate on the next investigation, which stays independent)
- Every investigation claim: Observation → Evidence → Reasoning → Conclusion → Confidence (see PHILOSOPHY.md rule 11)
- notes.md starts with a Prediction (guess/how/confidence/why) before the lesson, compared after
