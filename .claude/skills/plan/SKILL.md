---
name: plan
description: Run Eriks's personal planner — size the Todoist "Personal" board, fit tasks to the declared daily budget, and write an accepted plan back as section moves and due dates, one consent at a time. Use when Eriks says "plan my week", "plan my day", "plan the month", "/plan", "replan", "what should I work on at home", or "help me plan". Scoped to the personal instance folder; the work instance has its own.
---
INSTANCE_ROOT: the folder that holds `AGENTS.md` — the repository root. On
Eriks's laptop that is
`/Users/erik.peterson/Library/Mobile Documents/com~apple~CloudDocs/My AI OS/Assistant`;
in a cloud routine it is the working directory of the clone. (CHANGED
2026-09-19 from the fixed laptop path so the same skill runs unattended.)

Read `PLANNING.md` under INSTANCE_ROOT and execute its steps in order for the
horizon Eriks named (`month`, `week` or `day`). An ambiguous horizon is a
question, never a guess.

Constraints that are never improvised around:

- **Nothing is written anywhere** — Todoist, files, labels — before Eriks's
  explicit accept of the plan text in front of them.
- A due date is never invented outside an accepted plan, and a task that
  already has one is never rescheduled without Eriks naming it.
- Tasks carrying `someday` or `agent-waiting` are **never scored** — not
  sized, not planned.
- L-or-larger tasks are never auto-sized; unsized L+ candidates are asked
  about, and silence leaves them unsized and unplanned.
- The calendar is **read-only**; no event is ever created or modified.
- "Leave it for now" on any task is honoured immediately, with no push-back
  and no follow-up task.

Report at the end: counts (candidates / sized / planned / skipped), writes per
type with the verification result for each, what was not planned and why,
and anything defaulted or failed.
