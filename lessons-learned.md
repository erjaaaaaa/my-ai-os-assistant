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
