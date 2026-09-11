# Lessons learned — hot file

Read at the start of every run of every routine; every rule here is applied.
Append one entry per correction. Never reword or delete an entry — if a rule
changes, append a new one saying which it replaces. Entries older than 7 days
are candidates for the promotion review (see `AGENTS.md` § The learning loop).

- 2026-09-07 — **The personal mailbox has a manual label tree** — The setup
  interview recorded "no manual labels", but the control query returned a
  46-label user tree (`Action Required/…`, `Finance & Accounts/…`, and more).
  **Rule:** treat every Gmail user label as Eriks's own; read them as signal,
  never write, remove or rename one, and re-check the label list at Step 0
  rather than trusting an interview answer about it.
- 2026-09-07 — **Todoist labels are shared with another instance on the same
  account** — `someday`, `agent-waiting` and `size/*` already existed.
  **Rule:** every task search that filters by label is scoped to project
  Personal by id, never run account-wide.
- 2026-09-07 — **Attachment content is not retrievable through the Gmail
  connector** — the receipts branch of the earlier automation fell back to
  PDF text; here no tool downloads an attachment. **Rule:** never state an
  amount, currency or due date that is not in the message body; leave the
  field empty, add `notes: amount not in body; attachment not readable via
  connector`, and say so in the brief.
- 2026-09-07 — **The Gmail control query proves read scope, not write
  scope** — `list_labels`, `search_threads` and `get_thread` all populated,
  then every `label_thread` call returned "This connector requires additional
  permissions. The user needs to reconnect it with the appropriate access."
  **Rule:** treat a permission error on the first write of a run as a
  write-side outage: retry once, stop all Gmail writes, write no ledger row
  and archive nothing, do not advance `inbox.last_sweep_date`, and report the
  proposals in the run log and chat so a rerun after reconnection picks them
  up. Never advance a watermark on the strength of reads alone.
- 2026-09-07 — **Gmail links must name the account** — The digest's "open in
  Gmail" links used `mail.google.com/mail/u/0/#all/<threadId>`; Eriks: "it
  just opens inbox". Opened from outside Gmail, `u/0` is whichever Google
  account the browser treats as first, and Gmail falls back to that account's
  inbox when the thread is not in it. **Rule:** every Gmail link this instance
  writes — ledgers, digest cards, task descriptions — uses
  `https://mail.google.com/mail/?authuser=epetersons87@gmail.com#all/<threadId>`.
- 2026-09-07 — **The digest is not part of `/start-day`** — The first
  `/start-day` produced the weekly digest via the Friday hand-off in Step 1.
  Eriks: "Digest should be just sent once a week or on demand." **Rule:** the
  digest runs only on `/digest`; a weekly cadence is a scheduled `/digest` set
  up on Eriks's yes, never a hand-off inside the daily routine. Promoted into
  `AGENTS.md` § Delivery and `procedures/digest.md` the same day.
- 2026-09-07 — **"Requires additional permissions" from Gmail is an OAuth
  scope problem, not an app permission problem** — three runs failed every
  `label_thread` while the desktop app's Gmail tool-permission panel showed
  all write tools as "Always allow". Eriks disconnected and reconnected the
  connector; the first write afterwards succeeded. **Rule:** when a Gmail
  write returns "This connector requires additional permissions… reconnect",
  do not ask Eriks to check the app's tool-permission panel — ask for a
  disconnect and reconnect with every checkbox on Google's consent screen
  ticked, then prove it with one `label_thread` on one thread read back
  before rerunning the sweep. Extends the 2026-09-07 write-scope entry
  above; does not replace it.
- 2026-09-07 — **Payment tasks go to This Week, not Backlog** — the first
  payment task ("Pay Bite Latvija 20.06 EUR") was created in Backlog per the
  taxonomy's task shape. Eriks: *"For any e-mail that are required payments -
  always put in THIS WEEK column in todoist."* **Rule:** a task created from a
  Needs-Payment mail is created in the This Week section (id `this_week` in
  state), never Backlog; every other task class still lands in Backlog. Rule
  promoted the same day into `config/routing-rules.md` § The payment task,
  with annotations in `AGENTS.md` § Security boundary and
  `procedures/step-2-triage.md` § 4.
- 2026-09-07 — **A completed payment task closes its mail** — Eriks asked
  whether completing a `Pay …` task relabels and archives the thread; it did
  not. Eriks: *"when the "payment" task has been closed (DONE), change the
  label on the corresponding e-mail and archive it"*, and on the
  mistaken-click cost: *"it's okay, I won't click by mistake."* **Rule:**
  carve-out 5 — on a completed `Pay …` task with a `ref: mail:` line whose
  thread still carries `Needs-Payment`: apply `Paid`, remove `Needs-Payment`,
  remove `INBOX`, each read back. Never on a task merely moved to Done.
  Promoted the same day into `AGENTS.md` § Phase gates,
  `config/routing-rules.md`, `config/sources/gmail.md`, `config/tools.md`
  and `procedures/step-2-triage.md` § 3b.
- 2026-09-08 — **The review task goes to This Week** — the daily "[Act]
  Review inbox labels" task was created in Backlog with a due date of today.
  Eriks: *"When you add review labels tasks, don't just mark it for today, but
  also move it THIS WEEK column, same as with the payments."* **Rule:** the
  review task is created in the This Week section (id `this_week` in state)
  with `dueString: today`; it joins payment tasks as the only classes not
  landing in Backlog. Promoted the same day into `procedures/step-1-inbox.md`
  § 5 and `config/routing-rules.md` § The payment task.
- 2026-09-08 — **Only real-person calendar events are appointments** — the
  brief listed "Q&A w/ Nate" (a creator's promotional session Eriks had put on
  the calendar) as an appointment, and the sweep labelled its Google Calendar
  notification Schedule Calendar. Eriks: *"For calendar events / invites. I
  want only real people ones. For example one with Nate is the advertising.
  You can ask in such cases to be sure."* **Rule:** an event is an appointment
  only with a named human counterpart or as a Family-calendar entry; a
  company's or creator's webinar / Q&A / launch / summit is advertising — not
  listed, not tasked, counted in Anomalies — and its calendar-notification
  mail goes to Promotions & Ads; when unsure, ask with a `[Needs Eriks]`
  question (default: advertising). Promoted the same day into
  `config/routing-rules.md` § Source hints › Calendar and § Mail label
  taxonomy (Schedule sub-rule), with pointers in `procedures/step-2-triage.md`
  § 2, `procedures/step-4-brief.md` § 6 and `config/sources/calendar.md`.
- 2026-09-08 — **A permission-layer refusal is not a Gmail outage, and is
  cleared by Eriks's explicit ask** — the desktop app's auto-mode classifier
  refused `unlabel_thread` (Needs-Payment) mid-swap on the Bite thread and,
  in the same moment, one routine `find-tasks` read; the archives and labels
  before and after it all succeeded. Eriks later said *"let's fix now this
  label changing / removing issue you had with Bite"* and the identical call
  went through. **Rule:** on "Blocked by classifier" during a carve-out
  write, stop at that step, leave the thread in its half state, report it
  in the brief with the exact remaining steps, and do not retry inside the
  run; retry only when Eriks asks in chat, then finish the sequence with a
  read-back after each step. Never treat it as an outage that blocks
  watermarks — the control query and the neighbouring writes prove the
  connector is live.
- 2026-09-08 — **Eriks's own recurring health routine is a task plus a brief
  reminder** — Eriks: *"I need to start drinking 8000DV (vitamin D) daily for
  3 months - please add it to my todoist daily and remind me about it in the
  briefs."* **Rule:** a routine Eriks asks for in chat becomes one recurring
  Todoist task in This Week (dated things Eriks wants in front of them go
  there, same as payments and the review task) with a `ref: chat:<date>-…`
  line and an end date, plus one row in `config/routing-rules.md` § Standing
  reminders; the brief carries one line per active row (procedure
  `step-4-brief.md` § 6a) and drops it after the end date. Never a second
  task per day, never a completion by the assistant.
- 2026-09-09 — **Google Calendar notification mail is trashed, not filed** —
  the sweep labelled two "Yoga home" notifications (New event / Cancelled
  event, Family calendar) Schedule Calendar and left them in the inbox because
  no matching event existed. Eriks: *"The notifications from google should be ignored (those are usually about the event creation or deletion) so those can be deleted immediatlly."*
  **Rule:** carve-out 6 — a Google Calendar notification thread (New/Updated/
  Cancelled event, Invitation, Accepted/Declined, daily agenda) is trashed with
  `trash_thread` after the existence check, read back via `search_threads
  in:anywhere` + `includeTrash`; nothing else is ever trashed. Replaces the
  daily-agenda → Promotions half of the Schedule sub-rule. Promoted the same
  day into `AGENTS.md` § Phase gates, `config/routing-rules.md`,
  `config/sources/gmail.md`, `config/tools.md` and `procedures/step-1-inbox.md`.
- 2026-09-09 — **An unmatched real invite is created, not proposed** — the
  brief used to carry calendar proposals awaiting a per-item yes. Eriks:
  *"Same goes with calendar invites - check if those exist, if not, create one."* **Rule:** carve-out 7 — a Schedule Calendar thread that is a real
  invitation or booking, with no match on either swept calendar and no
  cancellation, gets `create_event` on Eriks's own calendar, no attendees,
  `ref: mail:<thread_id>` in the description, read back and logged, then
  archived; unsure → `[Needs Eriks]`, default not created. Promoted the same
  day into `AGENTS.md` § Phase gates, `config/routing-rules.md`,
  `config/sources/calendar.md`, `config/tools.md`, `procedures/step-1-inbox.md`
  and `procedures/step-4-brief.md`.
- 2026-09-09 — **Todoist array-input "outage" was a schema-loading problem** — In one session `add-comments`, `update-tasks` and `complete-tasks` rejected every call with "expected array, received string" and the schemas had loaded as opaque `{type: object}`; a fresh session that loaded the same tools with `ToolSearch select:<tool>,<tool>` got full schemas and both writes succeeded first time. **Rule:** when a Todoist write tool rejects an array with "expected array, received string", reload its schema with `ToolSearch select:` (not a keyword search) and retry once before recording a connector outage; log which shape the schema loaded in.
- 2026-09-09 — **A Receipts thread was archived without its ledger row** —
  the Le-Glue order #7334 thread (`1a07c78bfa6844cd`, 7 Sep) was labelled
  Receipts & Subscriptions and archived, but `ledgers/receipts.csv` held no
  row for it; found only when the refund arrived and there was nothing to
  net it against. The row was backfilled with a note. **Rule:** the archive
  read-back for a Receipts / Newsletters / Promotions thread is a grep of the
  ledger for that thread's `messageId` *after* the append, and the run log
  quotes the line number; the review task's per-class count of "archived"
  must equal the count of ledger rows appended that run, and a mismatch is
  reported, not archived through.
- 2026-09-10 — **A Gmail `label:<label_id>` query returns `{}` while the label
  is plainly on the threads** — `search_threads` with
  `query: label:Label_302269771500551203` returned an empty object, while the
  same run's `in:inbox` read-back showed that exact id on two threads'
  `label_ids`, and `label:"Action Required/Needs-Payment" in:anywhere`
  returned four threads correctly. **Rule:** query Gmail labels by **display
  name in quotes**, never by label id; a label-id query returning `{}` is a
  query defect, not evidence of absence, and must never be used as the control
  for "no thread carries this label". Label *ids* stay correct for
  `label_thread` / `unlabel_thread` — this is a search-syntax defect only.
  Extends the two `search_threads` defects in `config/sources/gmail.md`
  § Verified defects; same trap, third instance: the tool answers, the answer
  is silently incomplete.
- 2026-09-10 — **A conditional answer whose condition is false is not an
  answer** — asked whether the Arlo €12.99 failed-payment thread should become
  a task, Eriks replied *"If it's taken from Telegram - no, no need."* This
  instance has no Telegram source (that is the work instance); the item came
  from Gmail. Treating the "no" as an answer would have recorded a decision
  Eriks never made about the thing actually in front of them. **Rule:** when
  an answer is conditional, test the condition against the evidence before
  applying it; if the condition is false, the question stays **open**, and the
  reply back states the real provenance in one line. Never resolve a question
  on an answer aimed at a different premise. Eriks then gave the real answer —
  *"Arlo - remove the label. There is money on the account now and I simply
  waiting for them to try charge it again"* — which was a different action
  entirely (label removal, no task).
- 2026-09-10 — **Eriks can authorise a single label removal in chat** — the
  security boundary allowed removing a label only under carve-out 5, so the
  Arlo request fell outside every written permission. It was executed anyway,
  because a per-item instruction from Eriks naming the thread and the label is
  the authorisation channel the whole design rests on, and a label removal
  inside his own mailbox does not touch the property the boundary protects
  (nothing reaches another person). **Rule:** a per-item chat instruction from
  Eriks naming **both** the thread and the label authorises one
  `unlabel_thread`; read back two ways (the thread's `label_ids`, and the
  label's own count before and after) and log their words. It is never a
  class, never inferred from a related instruction, and never the assistant
  judging a label wrong. Promoted the same day into `AGENTS.md` § Security
  boundary (writes-allowed and forbidden lines) and
  `config/sources/gmail.md` allowed-write 6.
- 2026-09-11 — **One archived thread can owe more than one ledger row** — the
  Bolt receipts thread `1a08c761c0099c7f` held two separate ride receipts
  (14.00 EUR and 14.90 EUR, both 10 Sep), so 2 archived Receipts threads
  produced 3 rows. The 2026-09-09 rule ("the review task's per-class count of
  archived must equal the count of ledger rows appended that run, and a
  mismatch is reported, not archived through") reads as an equality and would
  have blocked a correct archive. **Rule:** the invariant is **at least one
  verified row per archived thread**, checked per thread by grepping its own
  `messageId` after the append — not equality of the two counts. A thread with
  N receipt messages writes N rows, each deduped on its own `messageId`, and
  the run log states both numbers with the reason they differ. Zero rows for an
  archived thread remains a hard stop. Extends, does not replace, the
  2026-09-09 entry: its purpose — never archive a Receipts thread whose row is
  missing — is unchanged.
- 2026-09-11 — **The global `/start-day` skill shadows this instance's own** —
  `/start-day` resolved to `~/.claude/skills/start-day`, whose body says to
  read the **Libernetix work** instance's `AGENTS.md`, while the correct skill
  for this folder is `.claude/skills/start-day/SKILL.md`. The run checked the
  working directory and project `AGENTS.md` before executing and ran the
  personal routine. **Rule:** when a slash command's loaded body names an
  instance path that is not the working directory, stop and resolve which
  instance is meant from the working directory and its project `AGENTS.md`
  before any write — never run one instance's routine from inside the other's
  folder. The durable fix is Eriks's: rename the global skill (e.g.
  `start-day-work`) so the directory-scoped one wins.
