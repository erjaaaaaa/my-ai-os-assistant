# Step 0 — Orient

Read at the start of every run. Nothing is written to any external system in
this step.

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

## Output of this step

A short orient block, kept for the run-log entry at Step 5: per source
live / outage / not configured with the containing totals; tracker status;
open-question counts; whether ids were re-resolved.
