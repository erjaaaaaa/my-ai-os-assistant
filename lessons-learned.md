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
- 2026-09-11 — **The `raw/` backlog was always the run's work; the default was
  wrong, not merely unchosen** — the open question asked whether `/start-day`
  should ingest the YouTube clippings Eriks saves into the vault's `raw/`, with
  the default *no, they stay yours*. Eriks: *"Ingest them. Isn't it part of the
  process?"* The question mark matters: he was not picking between two
  reasonable options, he was surprised the split existed. The defect was
  reading `procedures/step-3-ingest.md`'s purpose sentence ("snapshotting
  durable context from *this instance's* sources") as scoping the **ingest**
  half too, when it only ever scoped the **snapshot** half. **Rule:** a step
  with two mechanically different halves gets them named separately before any
  scope sentence is applied to either — here, *snapshotting* is conservative
  and source-scoped, *ingesting* is unconditional over the `raw/` root whatever
  put a file there. More generally: when a `[Needs Eriks]` question comes back
  answered **with a question**, treat the framing as the thing that was wrong
  and say so in the governing file, rather than recording the answer as a
  preference between the options offered. SETTLED into
  `procedures/step-3-ingest.md` § Boundaries the same day.
- 2026-09-11 — **A per-item chat instruction also authorises an archive, not
  just a label removal** — Eriks: *"Le-Glue refund received, we can archive the
  email and close the task"*, on a thread carrying **Reply/Do**, which is
  outside carve-out 4's four archivable classes. Executed, on the same
  reasoning the 2026-09-10 Arlo entry established: a per-item instruction from
  Eriks naming the thread is the authorisation channel the design rests on, and
  the act is inside his own mailbox, reversible, and reaches no other person.
  **Rule:** the per-item chat channel covers **mailbox-internal, reversible**
  acts on **one named thread** — currently one label removal (2026-09-10) and
  one `INBOX` removal (2026-09-11). It has never covered and does not cover
  trashing outside carve-out 6, spam, mark-read, renaming or deleting a label,
  or anything that leaves the mailbox; sending stays forbidden in every phase.
  Each use is read back and logged with Eriks's words. Promoted the same day
  into `AGENTS.md` § Security boundary (writes-allowed and forbidden lines) and
  `config/sources/gmail.md` allowed-write 7.
- 2026-09-11 — **Todoist typed parameters can degrade mid-session** — Eriks
  asked for two completed USG tasks to be reopened and moved to Waiting /
  Blocked. `uncomplete-tasks` rejected `ids` with *"expected array, received
  string"*; the 2026-09-09 rule was applied (reload with `ToolSearch select:`,
  retry once) and the schema came back opaque `{type: object}` again. A control
  proved it was not a connector outage and not specific to that tool:
  `find-tasks` with `limit: 100` had **succeeded at 08:41 in the same session**
  and failed identically at 09:2x, while `fetch-object` — string-only
  parameters — kept working and confirmed both tasks were still `checked: true`
  with nothing half-written. **Rule:** when a reload returns `{type: object}`,
  stop immediately — the whole Todoist server is unusable for typed arguments
  in that session, not just the one tool. Prove no partial write with a
  string-only read, write nothing, advance no watermark, log it, and tell Eriks
  a fresh session is needed. Do **not** substitute a different tool to achieve
  the same effect, and do **not** report the request as done. The 2026-09-09
  entry assumed the fault is present from session start; this one shows it can
  appear part-way through, so **a successful earlier write is not evidence the
  next one will serialise.**
- 2026-09-14 — **A hotel booking satisfies two taxonomy classes and the
  tie-break does not separate them** — the Booking.com Hotel Fisserhof
  confirmation (3–10 Jan 2027, €6,382.33) matched the **Travel - Bookings &
  Iterinary** test ("hotel bookings with itineraries … travel confirmations")
  and the **Schedule Calendar** test ("an explicit travel booking (itinerary,
  boarding pass)") equally well, while the stated tie-break ranks only
  *Travel > Loyalty > Receipts > Promotions* and is silent on Travel vs
  Schedule. The choice is not cosmetic: Schedule Calendar fires carve-out 7
  and would have created a seven-night event, Travel creates nothing.
  **Rule:** when two taxonomy classes fit and the tie-break does not order
  them, do not let the more specific-sounding label silently decide a
  *carve-out action* — label conservatively (the class whose post-action
  writes least), create nothing, and open a `[Needs Eriks]` question naming
  both readings and the write each would cause. A tie-break written to order
  *filing* must never be repurposed to authorise a *write*. Open as task
  `6hW4pGFRrff5M2FQ`.
- 2026-09-14 — **Carve-out 7 has no rule for a booking that is already in the
  past** — Tesla's test-drive confirmation (Sat 13 Sep 14:00) was a real,
  uncancelled booking on neither calendar, which is carve-out 7's literal
  trigger, but the appointment had already happened and the sender's own
  follow-up proved Eriks attended. Creating it would have put a stray past
  event on his calendar; the carve-out exists so a *future* commitment is not
  missed. **Rule:** a carve-out's trigger conditions are not its purpose —
  when an item satisfies the letter but defeats the stated reason for the
  permission, apply the carve-out's own documented fallback (here "unsure →
  not created") rather than either writing or inventing an exception, and ask.
  Note the second-order cost, which is why this needs answering: the thread
  then cannot be archived either, because carve-out 4 only permits archiving a
  Schedule Calendar thread "once handled", and handled is precisely what is
  undecided — so a withheld write leaves the item parked in the inbox. Open as
  task `6hW4pChWvwJ2jp5Q`.
- 2026-09-14 — **A classifier refusal mid-swap should stop the whole class,
  not just the one thread** — the 2026-09-08 rule says stop at the refused
  step and leave that thread in its half state. Two threads qualified for
  carve-out 5 this run; the first (Eco Baltia) was refused after `Paid` was
  applied. Starting the second would have blocked at the identical step and
  produced a *second* half-swapped thread, doubling the manual cleanup for no
  gain. **Rule:** when a carve-out write is refused by the permission layer,
  stop the remaining items in that same class for the run as well, and report
  them as owed rather than attempting them. Extends the 2026-09-08 entry,
  which addressed only the thread in hand; its own reasoning ("do not retry
  inside the run") applies with more force to a sibling item known to need the
  identical call.
- 2026-09-14 — **A half-finished carve-out is a defect, not a neutral pause** —
  the 2026-09-08 rule ("stop at the refused step, leave the thread in its half
  state, report it") was followed correctly when the classifier refused the Eco
  Baltia `unlabel_thread`, and the thread sat carrying **Paid + Needs-Payment +
  INBOX** until Eriks was asked. His reaction named the real cost: *"E-mail
  cannot sit at both Paid and Needs-Payment labels at the same time - it's
  confusing."* The stopping rule is right — retrying blind is worse — but
  reporting it as one bullet among fourteen brief sections undersold it.
  **Rule:** when a carve-out leaves an object in a contradictory state,
  report it as the **first** thing in the brief, state the exact remaining
  calls, and ask for the one word that finishes it — do not file it under
  Anomalies alongside cosmetic notes. The half state is a live defect in the
  mailbox for as long as it stands. Recorded in `config/routing-rules.md`
  § The payment task as a mutual-exclusion invariant.
- 2026-09-14 — **When a class answer and a per-item instruction arrive in one
  message, execute the end state, not the sequence** — Eriks wrote *"Youtube
  premium should be marked as Needs-payment since my payment failed. I've
  already updated the payment details … Can mark is as Paid."* Read literally
  and in order that is: apply Needs-Payment, then apply Paid — which would have
  produced, for two API calls, precisely the both-labels state he objected to in
  the previous sentence. **Rule:** a message that states a general rule *and* a
  disposition for the item in hand is setting policy for the future and an end
  state for the present; apply the rule to the config and the end state to the
  thread. Never perform an intermediate step whose only justification is the
  literal order of the sentences, especially when the same message forbids that
  intermediate state.
- 2026-09-14 — **Reconcile a census discrepancy before reporting, even a
  favourable-looking one** — after the swaps, `INBOX.threadsTotal` read 10 where
  15 − 2 archives = 13. The gap was Eriks trashing three notifications himself
  mid-conversation, confirmed by `in:anywhere` + `includeTrash` and corroborated
  three ways (TRASH +3, Professional Networking 140 + 4 − 3 = 141, and the
  Schedule Calendar count sitting at 275 rather than 279). **Rule:** a count
  that does not reconcile is investigated and explained in the report, never
  smoothed over or attributed to "the user probably did something" without a
  read that proves it. The same check incidentally established a fact worth
  keeping: **a trashed thread drops out of a user label's `threadsTotal`**, so
  label counts cannot be used as a labelling audit without accounting for
  trashes in the same window.
- 2026-09-17 — **The "already labelled → skip" check was run against a
  truncated message list** — `procedures/step-1-inbox.md` § 1.2 says to read
  `label_ids` off a thread's messages and skip the thread if any message
  carries one of the thirteen. The run read them off the `search_threads
  in:inbox` result, which returned **5 of the Luminor thread's 16 messages**
  with no truncation flag; ten of the eleven it hid already carried
  **Reply/Do** (and `Paid`) from Eriks's own June filing. The thread was
  therefore relabelled when it should have been skipped. No harm landed —
  the label applied was the same class the thread already held, so nothing
  is misfiled and nothing was archived on the strength of it — but the check
  was unsound, and a thread already sitting in a *different* class would have
  been silently given a second taxonomy label. The label census is what
  caught it: **Reply/Do's `threadsTotal` stayed at 97 while its
  `messagesTotal` rose 118 → 132**, which is only possible if the thread was
  already counted. **Rule:** the skip test is run against `get_thread`, never
  against a `search_threads` result — the search result decides *which*
  threads to look at and nothing else. Where reading every thread in full is
  the cost, that cost is the price of the test being sound. Second check,
  cheap and independent: after labelling, compare each label's `threadsTotal`
  before and after; **the sum of the per-label thread deltas must equal the
  number of `label_thread` calls made**, and any label showing +0 is a thread
  that was already in that class. This run: 21 calls, 20 net-new
  associations, one +0 — which is exactly how the defect surfaced. Fourth
  instance of the silent-truncation trap in `config/sources/gmail.md`
  § Verified defects, and the first where a *rule* rather than a *timestamp*
  was decided on the short list — the existing entry warns only that
  "`get_thread` matters for every watermark comparison", which reads as
  narrower than it is.
- 2026-09-20 — **A corrected re-send of the same invoice is one receipt, not
  two** — the getguru thread `1a0bb2f2a12eac0a` held two messages for invoice
  INV-092026-57274: 6.99 EUR, then 8.99 EUR once a 2.00 tip was added. The
  2026-09-11 rule ("a thread with N receipt messages writes N rows, each
  deduped on its own `messageId`") was written for a Bolt thread holding two
  *different* rides; applied literally here it would have booked one 8.99 EUR
  trip as 15.98 EUR in the ledger the digest and any spend question read from.
  **Rule:** before writing N rows for N receipt messages, compare their
  invoice numbers and the transaction they describe. Same invoice and same
  transaction → the later message **supersedes** the earlier one: write one
  row, for the final amount, and record the superseded messageId and figure in
  `notes`. Different transactions → N rows, as before. The hard invariant is
  unchanged and is what both branches protect: **at least one verified row per
  archived thread**, grepped by the thread's own id after the append. Extends,
  does not replace, the 2026-09-11 entry.
- 2026-09-20 — **Two oversized threads were classified without their bodies,
  and that needs saying out loud rather than quietly** — `procedures/step-1-inbox.md`
  § 1.3 says classify from the full thread text, not the snippet. Two bulk
  marketing threads this run (Xbox 229 KB, Biļešu Serviss 262 KB) were
  classified from `get_thread` metadata plus sender and subject: one because
  `bilesuserviss.lv` is named verbatim in the taxonomy's own sub-rule, the
  other because a sibling message from the identical Xbox campaign had been
  read in full minutes earlier. Both landed in Promotions & Ads, which is
  almost certainly right — but "almost certainly right" is how a skipped check
  always looks, and the 2026-09-17 truncation defect was exactly that shape.
  **Rule:** read the body by default; the cost of reading is the price of the
  check being sound. When a body is genuinely skipped, the run log must name
  the thread, say the body was not read, and state what carried the decision
  instead — never let a metadata-only classification be reported as if it were
  a full read. Whether the sender-named sub-rule cases should be a standing
  exemption is for the promotion review, not for the run that wants the
  shortcut.

- 2026-09-20 — **Read access is not write access, and a run that cannot
  record itself must not act** — the first cloud run (session
  `cse_01WLbEPgazXGPVFJdavv5kNW`) passed the routine's creation-time
  repository check, cloned and pulled fine, then labelled 33 threads,
  archived 26, wrote 25 ledger rows and created 3 tasks — and `git push`
  returned 403 because the Claude GitHub App had read but not write access.
  The archived threads leave the sweep for good, so their ledger rows and the
  advanced watermarks died with the container. The assistant had marked "app
  access granted" as done on the strength of a check that only proved reads.
  **Rule:** at Step 0 of every run with an `origin` remote, `git push
  --dry-run origin main` must succeed before any external write; if it is
  refused, the run stops and reports. And a permission is verified by
  exercising the exact operation the run needs (a write), never a weaker one.


- 2026-09-20 — **The review task cannot be updated, because the write
  allowlist has no description edit** — `procedures/step-1-inbox.md` § 5 says
  to upsert the day's `[Act] Review inbox labels` task, "search by title
  first; update rather than duplicate". The day's second cloud run found
  `6hXM3WFgxrC82HXQ` already created by the 00:22 run, and the only way to
  "update" it is to rewrite its description — which `AGENTS.md` § Security
  boundary does not list among the permitted Todoist writes (create, comment,
  apply labels, move sections, complete narrowly). The allowlist fails closed,
  so the second sweep's counts went in as a **comment**. **Rule:** a second
  run on the same date comments its counts onto the existing review task and
  never edits a description; and this disagreement between two governing files
  is a defect for the promotion review to settle — either the allowlist names
  description edits on the assistant's own tasks, or § 5 says "comment". Do
  not re-adjudicate it per run.
- 2026-09-20 — **A second run on the same date appends to the day's brief,
  never overwrites it** — `procedures/step-4-brief.md` says the brief is
  archived to `briefs/YYYY-MM-DD.md` and assumes one run per date. The 09:33
  UTC run found `briefs/2026-09-20.md` already holding the 00:22 run's brief,
  including its push-failure notice and the correction on top of it — all of
  which is record. **Rule:** when the file for today already exists, append
  the new brief below the old one under its own dated heading with a "(second
  run)" marker, on "supersede, never erase". Never truncate or rewrite a brief
  a previous run delivered, even one from the same day and the same routine.
- 2026-09-20 — **The same sender has been filed under three different
  taxonomy classes in four runs** — Skool (`noreply@skool.com`) went to
  Professional Networking on 17 Sep, Promotions & Ads on 20 Sep (first run),
  and Newsletters & Learning on 20 Sep (second run, a "Weekly digest for Sun,
  Sep 13 2026" from The RoboNuggets Network). Each reading is defensible on
  its own — a community notification, a promotional post, recurring editorial
  content — and the taxonomy's tie-breaks order only *Travel > Loyalty >
  Receipts > Promotions* and *Newsletters vs Promotions → Promotions*, neither
  of which reaches Professional Networking. The choice is not cosmetic:
  Newsletters and Promotions are archived with a ledger row, Professional
  Networking stays in the inbox. **Rule:** where a *sender* has been split
  across classes by previous runs, say so in the brief's Anomalies rather than
  quietly picking a third class, and grep the ledgers for that sender before
  classifying so the precedent is visible. A per-sender rule is Eriks's to
  set; until he does, classify the individual mail on its content and name the
  split. Sibling of the 2026-09-14 Travel-vs-Schedule entry, but about
  *consistency across runs* rather than one thread's tie-break.

- 2026-09-20 — **The "same campaign sent twice" case has a rule for receipts
  and none for promotions** — the Value Hunter thread `1a0bf527d2081c22` held
  two messages 1m44s apart, identical but for their sendfox tracking tokens:
  one marketing campaign, sent twice. `procedures/step-1-inbox.md` § 3 says
  for Newsletters/Promotions only "Dedupe on messageId", which yields two
  rows; the 2026-09-20 getguru entry, which would collapse them, is written
  for *Receipts* ("compare their invoice numbers and the transaction they
  describe") and a carve-out is never extended by analogy. Two rows were
  written, per the literal rule. The cost is real but small and cosmetic —
  the weekly digest shows that campaign twice — where the receipts version of
  the same mistake doubled a stated money amount, which is why that half got
  a rule first. The 17 Sep run hit this with the same sender and resolved it
  identically, so it is a twice-seen pattern, not a one-off. **Rule:** for
  Newsletters and Promotions, keep writing one row per messageId, and when
  two rows in one thread are the same campaign, say so in the run log and in
  the review-task comment so the digest's double-count is visible rather than
  silent. Whether § 3 should gain a same-campaign clause for these two classes
  is for the promotion review to settle — do not re-adjudicate it per run.
  Sibling of the 2026-09-20 getguru entry; neither replaces the other, and the
  hard invariant both protect is unchanged: at least one verified row per
  archived thread.

- 2026-09-20 — **The write-access gate can refuse for a reason that has
  nothing to do with write access** — the 20:00 UTC hourly cloud run found the
  clone on a **detached HEAD** at `4afd263` (exactly `origin/main`) with the
  local branch ref `main` still at `aa9e2da`, 19 commits behind. `git push
  --dry-run origin main` pushes the *branch ref*, not `HEAD`, so it was
  rejected **non-fast-forward** — and Step 0's NARROWED 2026-09-20 rule reads
  "if it is refused, the run stops here". Stopping would have been wrong: the
  gate was written for the first cloud run's **403**, where the GitHub App had
  read but not write access, and a non-fast-forward proves nothing either way
  about permission. The repair was local and lossless (`git checkout main` +
  `git merge --ff-only origin/main`, a pure fast-forward, nothing discarded,
  no force), after which the gate returned `Everything up-to-date`.
  **Rule:** distinguish the two refusals before applying the stop rule. A
  **403 / "permission denied" / "repository not found"** is the access failure
  the gate exists for — stop, report, write nothing. A **non-fast-forward or
  "behind its remote counterpart"** is local ref bookkeeping: check
  `git branch -vv` and `git rev-parse HEAD origin/main`, and if `HEAD` already
  equals `origin/main` and the tree is clean, fast-forward the local branch
  (never force, never rewrite someone else's history) and **re-run the gate**;
  only a second refusal stops the run. And because "Everything up-to-date"
  could in principle be answered locally, corroborate it once with
  `GIT_TRACE=1` showing `git-remote-https` was actually invoked — the point of
  the gate is that the permission is proven by exercising it, which a purely
  local answer would not do. Extends, does not replace, the 2026-09-20 entry
  above ("a run that cannot record itself must not act"): its purpose — never
  act before proving the run can be recorded — is unchanged; this narrows
  *which* refusals mean that.
- 2026-09-21 — **A Step 3 watermark of `0` read literally means "snapshot the
  whole mailbox"** — `procedures/step-3-ingest.md` § Finding mail to snapshot
  says the candidate set is *all mail newer than the Step 3 watermark*, and
  `vault.mail_snapshot.last_internaldate_ms` was still `0` because no mail
  snapshot had ever been written. Applied literally that is every thread in a
  ten-thousand-message mailbox. The run bounded the scan at the instance's own
  first run (`after:2026/09/07`) and named the bound in the brief as a default
  applied, not a decision. **Rule:** when a watermark is at its initial `0`, the
  first scan starts at the instance's first-run date, stated in the run log; and
  the Step 3 watermark advances to the newest thread the scan *returned and
  judged*, not only the newest one snapshotted — a thread excluded by label was
  processed, and re-reading it tomorrow is not the rule's purpose. Both halves
  belong in `procedures/step-3-ingest.md` via the promotion review.
- 2026-09-21 — **Gmail's `resultCountEstimate` is an estimate and can be off
  by nearly half** — `search_threads in:anywhere after:2026/09/07` reported
  `resultCountEstimate: 201` and then returned **380** threads across eight
  pages. **Rule:** never use `resultCountEstimate` as a containing total for the
  impossibility test; count the threads actually returned, paginated to
  exhaustion, and pair them with `list_labels` totals. Added to
  `config/sources/gmail.md` § Verified defects.
- 2026-09-21 — **The calendar connector dates an all-day event by the UTC
  day of its timestamps** — the first Hotel Fisserhof span was created from
  `2027-01-03T00:00:00+01:00` → `2027-01-11T00:00:00+01:00` and read back as
  `start.date 2027-01-02`, `end.date 2027-01-10`: one day early, and with
  `update_event` / `delete_event` forbidden the only remedy was a second,
  correct creation (`…T00:00:00Z` for the first day and the day after the
  last) plus a stray for Eriks to delete. **Rule:** for `allDay: true`, pass
  UTC-midnight timestamps for the first day and the day after the last day,
  and treat `start.date` / `end.date` on the read-back — never the request —
  as what was created. Recorded in `config/sources/calendar.md` § Verified
  defects. A carve-out 7 write that cannot be undone by the run deserves the
  read-back *before* the next dependent write, not after the batch.
- 2026-09-21 — **The write-access gate proves GitHub's permission, not the session's** — the 09:00 UTC hourly cloud run passed Step 0 § 0 cleanly (`git push --dry-run origin main` → `Everything up-to-date`, after the detached-HEAD repair), did its external writes — 2 labels, 1 archive, 1 ledger row, 1 Todoist comment — and then had `git push origin main` refused **locally** by the Claude Code auto-mode permission classifier: *"Reason: [Out-of-Place Publication]"*. Not a 403, not a non-fast-forward; GitHub was never reached. The 2026-09-20 gate was added so that a run which cannot record itself does not act, and it did not cover this: it exercises the *remote's* permission and is blind to the *session's own* permission layer, which sits in front of it and which `--dry-run` apparently does not trip. So the gate can pass and the push still not happen — the exact outcome the gate exists to prevent, reached by a different route. Archived threads leave the inbox sweep for good, so the ledger row for `1a0c31b42d54b34e` is again the thing at risk, exactly as in the first cloud run. **Rule:** treat a **local permission-layer refusal of the push** as its own third case alongside 403 (stop, report) and non-fast-forward (repair, re-run the gate): do not retry it inside the run, do not force, and do **not** route the commit around it via the GitHub MCP or any other write path — a refused push worked around is a refusal defeated, not satisfied. Record the failure in the run log with the **verbatim ledger rows and watermark values** needed to reconstruct the run, commit that, and end with the failure as the first line of the final message. And whether the gate should be strengthened to exercise a *real* push rather than `--dry-run` — the only check that would have caught this before the writes — is for the promotion review and for Eriks, since it means either granting the routine push permission or accepting that hourly runs act without recording. Extends, does not replace, the two 2026-09-20 git entries; their purpose — never act before proving the run can be recorded — is unchanged, and this shows the current proof is incomplete.
