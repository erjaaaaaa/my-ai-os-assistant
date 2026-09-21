# Step 0 — Orient

Read at the start of every run. Nothing is written to any external system in
this step.

## 0. Git sync and run context — ADDED 2026-09-19

If the instance has an `origin` remote, run `git pull --ff-only origin main`
before reading anything. A pull that cannot fast-forward means two runs
diverged: stop, report it, never force. Then decide the run context — chat
or cloud — per `procedures/cloud-run.md` § 1, read that file if the run is
unattended, and record the context in the orient block.

**ADDED 2026-09-21 — repair a detached HEAD before the gate.** Eriks,
answering `[Needs Eriks]` task `6hXfGMC78cv6vHpQ` with *"a)"*, where (a)
read *"Let Step 0 § 0 do the repair itself, before the gate — check out
`main` and fast-forward it to `origin/main` when `HEAD` already equals
`origin/main` and the tree is clean, then run the gate."* So, after the
pull and before the dry-run: if `git symbolic-ref -q HEAD` fails (detached)
and `git rev-parse HEAD` equals `git rev-parse origin/main` and `git status
--porcelain` is empty, run `git checkout main && git merge --ff-only
origin/main`; log one line. Never force, never rewrite history; any other
state is reported, not repaired. The gate then tests access and nothing
else, and its stop rule below applies to a **403 / permission / not-found**
refusal — a non-fast-forward after this repair is reported as an anomaly.
Superseded default: repair per run under the 2026-09-20 lesson.

**NARROWED 2026-09-20 — prove write access before any external write.**
After the pull, run `git push --dry-run origin main`. If it is refused, the
run **stops here**: it records the refusal in the run log and ends with the
refusal as the first line of its final message, and Steps 1–5 do not run.
Reason, from the first cloud run (session
`cse_01WLbEPgazXGPVFJdavv5kNW`): the routine labelled 33 threads, archived
26 and wrote 25 ledger rows, then `git push` returned 403 because the Claude
GitHub App had read but not write access. The archived threads never
re-enter the sweep, so their ledger rows were lost with the container. A
run that cannot record what it did must not do it.

## 1. Read the governing files

`config/routing-rules.md`, `config/tools.md`, `config/methods.md`, every file
in `config/sources/`, and `lessons-learned.md`. Not `config/planning-rules.md`
(planning reads it), not `config/voice-style-guide.md` (loaded only when
drafting).

## 2. Read state

`state/state.json`. Every id used later in the run comes from here. If the
tracker `_verified` date is more than 30 days old, re-resolve by name before
any write: `find-projects` with `searchText: Personal` (expect exactly one),
`find-sections` on that project, `find-labels` for the eight label names —
then update the ids and the date in state and note it in the run log. **If a
name resolves to nothing, stop and ask; never create a replacement.**

## 3. Health-check every source

For each file in `config/sources/`:

- `configured: false` → record **not configured**. Do not run the sweep. It is
  named in the brief's heartbeat as not configured. It is not an outage.
- `configured: true` → run its control query exactly as the adapter states.
  - Populated → **live**. Record the containing total the adapter names
    (Gmail: `INBOX.threadsTotal`; Calendar: the calendar count and the
    presence of both swept ids).
  - Empty or erroring → retry once, then record **outage**. Its sweep is
    skipped, its watermark will not advance, and the brief names it.

Then the tracker: `user-info` must return `epetersons87@gmail.com`. Anything
else is an outage of the commitment layer — report it and stop the run before
Step 2, because there is nowhere to write.

## 4. Read the answers to open questions — before any other step does work

Nothing else in the routine reads these comments, so this is the only place
the loop closes.

1. `find-tasks` with `projectId` from state, `labels: ["agent-waiting"]`,
   `limit: 100`, paginated to exhaustion.
2. `find-comments` on **each one** — every task, not a sample.
3. Distinguish Eriks's comments from the assistant's own. The connector posts
   as Eriks, so identity cannot tell them apart. Assistant comments follow a
   convention: they open with the bold marker `**Assistant —**` and carry a
   `ref:` line. **Anything else on one of these tasks is Eriks answering.**
4. A comment from Eriks is an answer. Apply it — update whichever governing
   file the answer changes, quoting Eriks's words with the date — verify the
   edit by reading it back, **then complete the task** with `complete-tasks`.
   This is the one task class the assistant may complete itself.
5. An ambiguous comment → reply in the task (with the marker), narrowing the
   question; leave it open. Never apply a misread answer to a config file.
6. Silence → the stated default stays in force and the task stays open.

Record the count: open, answered-and-closed, ambiguous.

## 5. Idempotency check

If `calendar.last_scanned_date` already equals today and the mail sweep
finds nothing newer than `mail.last_internaldate_ms`, the run is a rerun:
Steps 2 and 3 no-op and the brief says so.

**NARROWED 2026-09-19 — Step 3 is exempt from the rerun no-op** when the
vault is reachable and there is pending work: the `raw/` root is not empty,
or mail newer than `vault.mail_snapshot.last_internaldate_ms` exists. A
cloud run defers Step 3 (`procedures/cloud-run.md` § 2.3), so the next
laptop run on the same day must catch it up. Superseded text: "Steps 2 and
3 no-op" — Step 2 still no-ops; Step 3 no-ops only when nothing is pending.

## Output of this step

A short orient block, kept for the run-log entry at Step 5: per source
live / outage / not configured with the containing totals; tracker status;
open-question counts; whether ids were re-resolved.
