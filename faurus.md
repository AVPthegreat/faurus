# Faurus — Concept Note

**One proctored pipeline for every technical assessment.**

Live: [faurus.app](https://faurus.app) · Platform: [code.faurus.app](https://code.faurus.app) · Exam portal: exam.faurus.app · Hiring: hiring.faurus.app
Stage: live production, real users · Built solo (engine) + Crew content · India-first B2B SaaS (edtech)

_Last verified against production: 2026-09-28._

---

## The problem

Technical assessment is fragmented and fake. A candidate's coding, theory, MCQs, and viva get split across five tools that don't talk to each other. Many "grade" code by pattern-matching instead of actually running it, and proctoring is theater that students can bypass. Institutions can't trust the scores, and honest candidates lose to gamed ones.

## What Faurus is

A single proctored pipeline covering a CS student's entire four-year journey — from first line of code to job-ready. Coding, theory, MCQ, numerical, viva, and interview rounds in one platform, built for universities, coaching institutes, and hiring teams. Every answer that can be executed *is* executed, against real engines with hidden test cases.

## What's live right now (not a mockup)

**Auto-graded execution engines, each a real sandboxed grader:**

| Engine | Status |
|---|---|
| DSA (Java / C / C++ / Python) | Live |
| SQL (real MariaDB, result-set graded) | Live |
| MongoDB (real instance per submission) | Live |
| Redis | Live |
| Node.js + full-stack Project (hidden test suite) | Live |
| Bash + Git (state-based grading) | Live |
| HTML / CSS (headless-Chromium renders + asserts structure/style) | Live |
| **Data Science** — 155 problems, numpy/pandas/sklearn/scipy/matplotlib + torch-CPU/nltk/opencv, Kaggle-style competition leaderboard | Live, complete |
| **OOP / Design** — 50 problems, hidden per-language unit-test harness (JUnit5 / doctest / pytest), per-method grading | Live |
| **Regex** — 12 problems, graded against hidden must-match / must-reject sets | Live |
| **Linux / sysadmin** — 10 problems, graded on resulting filesystem/permission/process state | Live |
| MCQ / theory / numerical / subjective | Live |

**Assessment + proctoring:** MCQ, numerical, theory, image and video formats; viva scheduling with AI-assisted evaluation; desktop app + browser-extension proctoring; ACM and OI contest scoring modes.

## Traction (live, pulled directly from production)

These are real numbers from the platform's public `/live` page ([code.faurus.app/live](https://code.faurus.app/live)) — nothing on that page is hard-coded, every figure is a live DB query refreshed every 45s.

| Metric | Value |
|---|---|
| Total users | **9,065** |
| Total submissions | **134,802** |
| Public problems | **967** |
| Submissions today | 106 |
| Active users (last 15 min) | 7 |
| Being judged right now | 32 |

That's ~57,000 new submissions and ~120 new users since the last documented snapshot — organic, ongoing usage on live production, not a demo dataset.

## Proof it holds under real exam load (measured on live production)

On a single 16-core AWS box (m7i.4xlarge, Mumbai), with all 15 scaling fixes deployed and verified inside running containers:

| Burst | Wall time | Effective rate | Backlog at end | Correctness |
|---|---|---|---|---|
| 1,000 submissions | 32.4 s | 30.84 sub/s | 0 pending | 100% |
| 2,500 submissions | 80.5 s | 31.05 sub/s | 0 pending | 100% |

Peak host load during the 2,500 burst was ~40% (6.25 of 16 cores) — comfortable headroom. The judge fleet never fell behind: at the instant the last submission was dispatched, everything was already judged. A full 2,500-student closing rush is a non-event.

The judge tier is stateless and Redis-coordinated, so it scales close to linearly per added node toward the north-star target of 100k concurrent users / 20–30k submissions/sec.

## Why now

Hiring and exams have gone remote, but the tooling still fakes grading and proctoring — scores nobody trusts, cheating nobody catches. Colleges bring 3,000+ students each, and a full cohort sitting one contest simultaneously must not degrade. The gap between what institutions need and what existing tools deliver is wide open.

## Why me

I didn't come to this from a slide deck — I built the hard parts alone: the judge that truly runs the code across a dozen engines, the proctoring, the scoring, and the load tests proving it holds under a full exam rush on a single box. My non-negotiable: if a platform mis-scores an exam or leaks a late submission, it has no right to exist.

## Competitive position

No verified competitor ships first-class MongoDB + Redis + Git-state graders together with HTML/CSS headless grading and a data-science competition leaderboard. Adding OOP / Docker / Linux / Regex / API testing likely makes Faurus the broadest platform by engine count. A mainstream Security/CTF engine — which nobody offers — is a genuine differentiator on the roadmap.

## Where it's going

- **Tier-1 engines remaining:** Docker/containers, API/backend testing.
- **Tier-2 engines:** compiler/automata, concurrency, functional, assembly, security/CTF.
- **Content & language expansion:** a language-agnostic "Basics" library, DSA in JS/Rust/Go/Ruby, exam & contest performance analytics with candidate reports, assessment presets, bulk candidate invites, and plagiarism detection.
- **Scale:** multi-node judge fleet toward 100k users / 20–30k sub/s.
