# Planning — month, week, day

The planning procedure for the Personal project, run by `/plan`. Consent-gated
throughout: **nothing is written anywhere before Eriks accepts the plan text
in front of them.** Policy — sizes, the capacity formula, the full-label-set
rule, what is never scored — lives in `config/planning-rules.md`; read it
first and do not restate it.

Horizons: `month`, `week`, `day`. An ambiguous horizon is a question. A day
plan inside an accepted week plan works from that week's list; a week plan
inside an accepted month plan works from that month's.

## Step 0 — Orient

Read `config/planning-rules.md`, `config/methods.md`, `lessons-learned.md`,
`state/state.json`, `state/planning-state.json`, and the most recent accepted
plan for the enclosing horizon in `plans/` if one exists. Run the Todoist and
Calendar control queries. Tracker outage → stop; calendar outage → continue
with `calendar: UNREACHABLE` in the header.

## Step 1 — Read the board

`find-tasks` on project Personal, `limit: 100`, paginated. Read open
`agent-waiting` comments first (the Step 0 rule in
`procedures/step-0-orient.md` applies here too — answers are applied before
anything is planned). Then partition:

- **Never scored:** `someday`, `agent-waiting`.
- **In place only:** subtasks (sized and dated under their parent, never
  moved).
- **Candidates:** everything else, top-level.

Census: candidates + never-scored + subtasks = open total. State it.

## Step 2 — Size

For each unsized candidate, apply the heuristic. XS/S/M: propose the label.
L or larger: **do not guess** — collect into one question block: title,
proposed size, why. Ask once, in chat, listing every L+ candidate with a
default of *unsized, not planned*. Silence leaves them unsized. Present the
proposed XS/S/M labels for acceptance together with the plan (Step 5); they
are written only then.

## Step 3 — Capacity

From `config/planning-rules.md`: per-day capacity, multiplied out for the
horizon. Read the two swept calendars for the horizon (conflicts and
appointments that need prep; **not** capacity). Print the arithmetic in the
plan header: `capacity <n>m = <budget> × <fraction> × <days>`.

## Step 4 — Rank and fit

Rank by: a stated due date inside the horizon; a named person waiting; Eriks's
priority; then age. Fit sized tasks to capacity in that order, reserving one
protected block per day for the largest placed item. Everything that does not
fit, is unsized, is `size/XXL`, or is never-scored goes under **"Not planned,
and why"** with its reason. Print `planned <n>m / capacity <m>m`; if
over-committed because of a commitment someone is waiting on, print the
overshoot rather than adjusting either number.

## Step 5 — Propose

Show the plan as text: header (horizon, dates, capacity arithmetic, calendar
status), the placed tasks in order with size and target section (month → This
Month; week → This Week; day → In Progress for the first item, This Week for
the rest) and any due date being proposed, the proposed XS/S/M labels, and
"Not planned, and why". Then ask for acceptance.

**What is a yes:** "yes", "apply", "do it", "looks good", or Eriks naming the
plan and telling you to proceed. **What is not:** silence, "later", "maybe", a
follow-up question, or approval of a different item. Any edit Eriks gives is
folded in and the amended text is shown again.

**"Leave it for now" on any task** is honoured immediately: the task drops
out of the plan with no push-back, no re-ask, and no `[Needs Eriks]` task.

## Step 6 — Write, one verified write at a time

Only after a yes. For each placed task: re-read its labels (`fetch-object`),
`update-tasks` with the complete label set and the target `sectionId` (never
a task with a `parentId`), and `reschedule-tasks` for a due date **only** for
tasks the accepted text showed with that date and that had none before. Read
each task back and diff. A failed verification stops the batch and is
reported as a failure.

A due date already on a task is never changed unless Eriks named that task and
that change. No task is ever completed, cut or deleted by a plan.

## Step 7 — Record

Write `plans/YYYY-MM-DD-<horizon>.md` with the accepted text verbatim plus a
"Written" block listing each write and its verification. Update
`state/planning-state.json` (`last_plan`, the horizon's date, the capacity
used). Append a line to `logs/run-log.md`. Commit.

## Step 8 — Report

Counts: candidates / sized / planned / not planned; writes per type with the
verification result for each; what was left unsized and why; anything
defaulted or failed.
