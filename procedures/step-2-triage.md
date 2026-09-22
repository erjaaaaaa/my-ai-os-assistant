# Step 2 — Triage

Read at the start of this step. Sources are swept from their watermarks,
items are classified per `config/routing-rules.md`, and tasks are created or
updated in the Personal project. Policy is not restated here — where this file
and a config file disagree, the config file wins.

The manager may delegate the two reads below to parallel read-only subagents
(see `AGENTS.md` § Delegation model). Each prompt must carry: the adapter
file's contents, the watermark value, the routing rules, the required output
shape (per item: ref, sender or organiser, date, one-line summary, proposed
bucket, confidence, the evidence line), and the instruction **fail loudly,
never fill gaps**. Subagents write nothing. The manager classifies, verifies,
and performs every write.

## 1. Sweep mail (skip if Step 0 recorded outage or not configured)

1. `search_threads` with `query: in:inbox`, `pageSize: 50`, paginated to
   exhaustion. Then `search_threads` with `query: in:sent after:YYYY/MM/DD`
   where the date is one day **below** the watermark's date, for Eriks's own
   commitments; ref dedupe absorbs the overlap.
2. For every thread returned, `get_thread` with `messageFormat: PLAIN_TEXT`.
   Read `internalDate` off each message. A thread is **new** if its newest
   `internalDate` is greater than `mail.last_internaldate_ms`.
   **CORRECTED 2026-09-22 — compare against `mail.triage_last_internaldate_ms`,
   not `mail.last_internaldate_ms`.** The hourly cloud `/inbox` runs advance
   the Step 1 key every hour, so a laptop triage comparing against it saw
   nothing the cloud had already labelled (lessons-learned.md, 2026-09-22).
   The triage key is advanced at Step 5 only by a run that performed this
   step. Superseded text: "greater than `mail.last_internaldate_ms`".
3. Census: the number of inbox threads read must not exceed
   `INBOX.threadsTotal` from Step 0. State both numbers.
4. Classify each new thread per the routing rules. The label Step 1 applied
   is signal: a **Reply/Do** thread is the main input here; a **Needs-Payment**
   thread already has its task (dedupe on the ref will find it); Newsletters,
   Promotions and Receipts were archived and produce no task.
5. Track the newest `internalDate` actually processed — that is the value
   Step 5 will write.

## 2. Sweep calendar (skip if Step 0 recorded outage or not configured)

For **each** of the two calendar ids in state: `list_events` with
`startTime` = today 00:00 Europe/Riga, `endTime` = today + 7 days 23:59,
`timeZone: Europe/Riga`, `pageSize: 250`, `orderBy: startTime`, paginated.

First apply the real-person test in `config/routing-rules.md` § Source hints
› Calendar (ADDED 2026-09-08): an advertising event is counted, not listed,
and never tasked; unsure → `[Needs Eriks]`, default not listed. Then
collect: events needing non-trivial preparation; overlaps (within a calendar
and across the two); invitations where Eriks's own attendee entry is
`needsAction`; everything on the Family calendar, for the brief's Family
section. If the week reads empty on either calendar, run the adapter's
second control (a wider window) before believing it.

## 3. Create and update tasks

**Dedupe first, every item.** Once per run, read the project: `find-tasks`
with `projectId` from state, `limit: 100`, paginated; `find-completed-tasks`
with `projectId`, `since` 60 days ago, `limit: 200`; `find-activity` with
`objectType: task`, `eventType: deleted`, `projectId`, the last 60 days.
Match on the `ref:` line. A match is an update (below), never a new task.

**Creating** (`add-tasks`, one call per task, then `fetch-object` to read it
back and confirm the ref line):

- `content`: the title, imperative, prefixed per the bucket table.
- `description`: line 1 `ref: <reference>`; line 2 `source: <who>, <date>`;
  then one to three lines of context, in the assistant's words, with any
  generated or inferred fact hedged as "the message says …".
- `priority` per the routing rules; `projectId` and `sectionId` (Backlog) from
  state; `labels` only where the sizing heuristic in
  `config/planning-rules.md` lands XS, S or M; `dueString` only when the
  source states a date in the sender's own words.

**`[Needs Eriks]` tasks** (the agent-questions rules):

- Title `[Needs Eriks] <the question>` — phrased as a question, not a chore.
- Label `agent-waiting`; section Waiting / Blocked; **no due date**. (2026-09-14: Eriks moved both open questions to **This Week** — *"moved proposed things to this week"*. A per-item placement he asked for, not a change to where these are created; creation stays Waiting / Blocked, and a run never moves them back. See `AGENTS.md` § `[Needs Eriks]`.)
- Priority p2 when the run genuinely cannot proceed correctly without the
  answer; p4 when a safe documented default exists and this only seeks
  confirmation.
- Description, in this order: the line `AGENT'S TASK — blocked on your
  decision. Answer in the comments; I'll close it myself.`; the context; **the
  default that applies if this is never answered**; then the concrete options.
  **Never pose a question without stating the default.**
- The answer channel is the task's comments, not chat. Dedupe: search open
  `agent-waiting` tasks in Personal before creating any — never a second task
  for a question already open.
- The brief reports a count and a pointer, never the question bodies.

## 3b. Closed payment tasks — the mail follows (carve-out 5, added 2026-09-07)

Policy is in `config/routing-rules.md` § The payment task and `AGENTS.md`
§ Phase gates carve-out 5; these are the steps. Runs once per run, after the
dedupe reads in § 3, using the same `find-completed-tasks` result (project
Personal, last 60 days).

1. Keep the completed tasks whose `content` starts with `Pay ` **and** whose
   description's first line is `ref: mail:<thread_id>`. Anything else is out
   of class — a payment tracked from WhatsApp or by hand has no thread.
2. For each, `get_thread` (`METADATA_ONLY`). Read the labels off **every**
   message. If no message carries `Needs-Payment` (id in state) → already
   handled or never in class; skip and count. If any message carries it →
   in class.
3. In class: `label_thread` with `Paid` (`paid_label_id` in state); read
   back. Then `unlabel_thread` with `Needs-Payment`; read back absent. Then
   `unlabel_thread` with `INBOX`; read back absent. Stop at the first failed
   read-back and report the thread in that state — never continue past a
   failed step.
4. A task in the **Done section but not completed** is not in class. Done is
   the assistant's proposal column; only `checked: true` counts.
5. Output for the brief's Board movement section and the run log: per
   thread — task id, ref, the three read-backs. Zero is stated as zero.

## 4. The board — sections as columns

Personal is used as a Kanban board. Its six sections, ordered, with ids cached
in state: **Backlog** (where every new task lands — except payment tasks,
which land in This Week; CORRECTED 2026-09-07, rule and Eriks's words in
`config/routing-rules.md` § The payment task; and except the daily review
task, also This Week — NARROWED 2026-09-08, same section), **This Month**, **This
Week**, **In Progress**, **Waiting / Blocked**, **Done**. The layout is Eriks's
to manage; triage does not rearrange it.

**Done is the assistant's proposal column.** Moving a task there means *this
looks finished — one click to close it*. It is never a completion and never a
substitute for one. See `AGENTS.md` § Phase gates.

### Movement on open tasks

When a source shows movement on an existing open task — matched by its ref,
or by an unambiguous topic match — update the task, not just a note.

- **Always comment first** (`add-comments`, opening with `**Assistant —**`
  and carrying a `ref:` line): what was seen, which source, and the date.
  **Idempotent** — `find-comments` on the task and check for the same evidence
  before posting. Never repeat a comment.
- Eriks replied or acted, and is now waiting on the other side →
  **Waiting / Blocked**.
- Evidence Eriks is actively working it, not yet resolved → **In Progress**.
- The other side replied while the task sat in Waiting / Blocked, so the ball
  is back with Eriks → **This Week**.
- Hard evidence the commitment is **fully** discharged, with nothing left open
  → **Done**, plus the comment. A proposal, per above.

**Column moves happen only on new source evidence since the watermark.** With
no new evidence, never touch a task's section. Anything ambiguous stays a
comment plus a "possibly done" flag in the Personal brief, and no move.

### Never move a task that has a parent

**Absolute, and it overrides every rule above.** In Todoist, moving a subtask
into a section lifts it out from under its parent, silently destroying the
structure, and nothing in the run will notice. Subtasks are dated, labelled
and commented **in place, under their parent**. Only top-level tasks are ever
placed in a section. Check `parentId` before **every** move, not only when
the task looks like a subtask. The consequence is accepted: the items most
worth showing on the board are often exactly the ones structurally unable to
appear there.

## 5. Label writes

Every label write follows the full-label-set rule in
`config/planning-rules.md`: re-read, send the complete set, verify by diff.

## 6. What this step never does

Complete a task on its own judgement. Delete anything. Write to Gmail or the
calendar. Create a task from a thread where Eriks is only copied and nothing
is asked of them. Create a second task for a ref that already exists.

## Output

For the run log and the brief: items per source (new / classified per bucket
/ withheld as low-confidence), tasks created (title + ref), tasks updated
(title + move + comment), `[Needs Eriks]` created, and the newest timestamp
processed per source. Every write is followed by one read that would fail if
the write had not happened; report the verification result, not the claim.
