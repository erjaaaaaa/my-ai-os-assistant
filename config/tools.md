# Tool registry

**Keep-current clause.** If a run discovers drift — a listed tool is gone, a
tool is in use that is not listed here, or a call shape below turned out to be
wrong — fixing this file is part of that run, and the fix is logged.

Connector tools appear as `mcp__<instance-id>__<tool>`. **The instance id
changes on reconnection; the tool name does not.** Find tools by keyword
search and match on the tool name. Never write an instance id into any file.

Any connector or tool the session has loaded that is not listed here is not
used by this instance, whatever it is.

## Todoist — the tracker

- **Connected:** desktop-app connector, authenticated as Eriks's own account
  (`user-info` returns `epetersons87@gmail.com`).
- **Used by:** Step 0 (read), Step 1 (read/write), Step 3 (read), PLANNING
  (read/write), ad-hoc chat (read; write only on Eriks's explicit yes).
- **Read:** `user-info`, `find-projects`, `find-sections`, `find-labels`,
  `find-tasks` (always pass `limit`, max 100, and paginate on `cursor`),
  `find-completed-tasks`, `find-activity`, `find-comments`, `fetch-object`.
- **Write, project Personal only:** `add-tasks`, `add-comments`,
  `update-tasks` (labels — full-label-set rule in `config/planning-rules.md`;
  section moves; due dates only inside an accepted plan), `complete-tasks`
  (only an `agent-waiting` task once answered, or a task Eriks named with a
  per-item yes), `reschedule-tasks` (only inside an accepted plan; never on a
  recurring task).
- **Never used:** `delete-object`, `add-projects`, `update-projects`,
  `add-sections`, `update-sections`, `add-labels`, `update-labels`,
  `manage-assignments`, `project-move`, `import-project-template`,
  `uncomplete-tasks`, any write to a project other than Personal, any edit to
  one of Eriks's own labels (`book`, `health`, `call`, `life`, `plan`,
  `admin`, `home`, `buy` and the rest).
- **If broken:** report the outage in the brief and stop all writes. Never
  reconstruct a task list from files or memory.

## Gmail — a source

- **Connected:** desktop-app connector, personal mailbox (the label tree
  returned by `list_labels` is Eriks's personal one).
- **Used by:** Step 0 (control), Step 1 (read), Step 2 (read, for snapshots),
  drafting on request (write: draft only).
- **Read:** `search_threads`, `get_thread` (`PLAIN_TEXT`), `get_message`,
  `list_labels`.
- **Write:** `create_draft`, `update_draft` — only when Eriks asks for a draft.
- **Never used:** `send_message`, `reply`, `forward`, `label_thread`,
  `unlabel_thread`, `label_message`, `unlabel_message`,
  `update_message_labels`, `create_label`, `update_label`, `delete_label`,
  `trash_thread`, `trash_message`, `mark_thread_spam`, `mark_message_spam`,
  `apply_sensitive_*`.
- **If broken:** outage; watermark does not move; brief names it.

## Google Calendar — a source

- **Connected:** desktop-app connector, same Google account.
- **Used by:** Step 0 (control), Step 1 (read), PLANNING (read, for conflicts
  and prep only — capacity is a declared budget, not a calendar derivation).
- **Read:** `list_calendars`, `list_events`, `get_event`, `search_events`.
- **Never used:** `create_event`, `update_event`, `delete_event`,
  `respond_to_event`, `suggest_time` (a per-request booking Eriks authorises
  in chat is a logged one-time exception, never a standing permission).
- **If broken:** outage; scanned-date key does not move; brief names it.

## Filesystem

- **This folder** — read/write. `state/state.json` holds ids, watermarks,
  flags; `logs/run-log.md` is append-only; `briefs/` and `plans/` are the
  record.
- **The vault `../My Brain/`** — declared in `.claude/settings.json`.
  Read anywhere; write **only through the ingest workflow** in
  `procedures/step-2-ingest.md`. `raw/processed/` is never edited.
- **Git** — the instance is a repository. Backups are commits, not `.bak`
  files.

## Diagnostic sequence for a missing connector

1. Search the session's tools for the stable tool name. Absent entirely →
   `needs-connecting`: report as **not configured** if the adapter says
   `configured: false`, otherwise as an outage (it was configured and is now
   gone).
2. Present but the control query errors or returns empty → `broken`. Retry
   once. Still empty → outage, reported as one. Do not advance the watermark.
3. Never substitute a different tool, a cached result, or memory for the
   missing one.

## Verified facts and defects — measurement → consequence → rule

- **2026-09-07 — Todoist labels are account-wide.** `find-labels` returned 55
  personal labels including `someday`, `agent-waiting` and the six `size/*`
  labels, which another instance on the same account also uses. → An
  unscoped `agent-waiting` search returns another instance's questions. →
  **Always scope task searches to project Personal by id.**
- **2026-09-07 — `find-tasks` defaults to `limit: 10`.** → A default listing
  silently truncates and looks complete. → **Always pass `limit: 100` and
  paginate on `cursor` until `hasMore` is false.**
- **2026-09-07 — `list_calendars` returned two calendars both named
  "Todoist".** → They mirror tracker tasks; sweeping them double-counts. →
  **Ignored; only the two calendar ids in state are swept.**
- **2026-09-07 — Gmail `list_labels` reported `INBOX.threadsTotal: 24`.** →
  The whole inbox fits one sweep. → **No per-run cap until it passes ~200;
  the census line in every brief reports the sweep count against
  `threadsTotal`.**
- **2026-09-07 — The Personal project already had six sections including an
  explicit `Backlog`.** → The board's backlog is that section, not "no
  section". → **Newly created tasks land in the Backlog section by id.**
