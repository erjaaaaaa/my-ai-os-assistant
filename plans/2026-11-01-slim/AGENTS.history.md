# AGENTS.history.md — relocated passages of AGENTS.md

DRAFT for Eriks's review (plans/2026-11-01-slim/). **Not read at the start of a run.**
Every block below is copied **verbatim** from the working copy of `AGENTS.md` of 2026-11-01
(693 lines; it differs from git HEAD only in lines 616–621, the uncommitted
"CHANGED 2026-11-01" paragraph, which is inside the §R block). Each header gives the
original line range. For CONDENSED rows the block is the full original range, so it also
repeats text that stays live in `AGENTS.slim.md`; for MOVED rows the text is gone from the
slim file. Per § Supersede, never erase: nothing here is reworded.
Section ids (§S, §C4 ...) are the pointers used in `AGENTS.slim.md`.

## §S — From AGENTS.md lines 108–108 (Security boundary — Writes allowed)

**Writes allowed:** Todoist, project Personal only — create tasks (in Backlog; NARROWED 2026-09-07 by Eriks: payment tasks from Needs-Payment mail are created in **This Week** instead — *"For any e-mail that are required payments - always put in THIS WEEK column in todoist"*; the rule is in `config/routing-rules.md` § The payment task; NARROWED again 2026-09-08 by Eriks: the daily review task also goes to **This Week** — *"same as with the payments"*, rule in `config/routing-rules.md` § The payment task), comment, apply labels under the full-label-set rule, move top-level tasks between sections on new evidence, and complete a task only when it is the assistant's own `[Needs Eriks]` task or Eriks has said yes naming that individual item. Gmail — apply exactly one of the thirteen labels in `state/state.json` to an unlabelled inbox thread; remove `INBOX` from a thread in one of the four carve-out classes in § Phase gates; under carve-out 5 there, apply `Paid`, remove `Needs-Payment` and remove `INBOX` on a thread whose payment task Eriks completed (WIDENED 2026-09-07, Eriks); under carve-out 6, `trash_thread` on a Google Calendar notification thread (WIDENED 2026-09-09, Eriks); **remove one named label from one named thread when Eriks says so in chat, naming both** (WIDENED 2026-09-10, Eriks, on the Arlo failed-payment thread `mail:1a064a1a38308887`: *"Arlo - remove the label. There is money on the account now and I simply waiting for them to try charge it again"*) — a per-item instruction only, never a class, never inferred, and never the assistant's own judgement; **apply `Paid` to one named thread when Eriks says so in chat** (WIDENED 2026-09-14, Eriks, on the YouTube Premium thread `mail:1a099e498f6dc9a7`: *"I've already updated the payment details and it went though all fine. Can mark is as Paid."*) — the second place `Paid` is written outside carve-out 5, a per-item instruction only, never a class, never the assistant judging a payment settled; **remove `INBOX` from one named thread outside the four carve-out classes when Eriks says so in chat, naming the thread** (WIDENED 2026-09-11, Eriks, on the Le-Glue thread `mail:1a0813348da28051`: *"Le-Glue refund received, we can archive the email and close the task"*) — the same per-item channel as the label removal above, and bounded the same way: one named thread, never a class, never inferred, never the assistant deciding a thread is finished; a draft, only when Eriks asks for one. Google Calendar — `create_event` on Eriks's own calendar, without attendees, only on Eriks's per-item yes naming that event (WIDENED 2026-09-09 by Eriks into a standing rule, carve-out 7: also without a per-item yes when a Schedule Calendar thread's event exists on neither swept calendar and carries no cancellation — *"Same goes with calendar invites - check if those exist, if not, create one."*). The vault at `../My Brain/` — through the ingest workflow in `procedures/step-3-ingest.md` only. This folder's own files: state, logs, briefs, plans, ledgers, config.

## §C4 — From AGENTS.md lines 286–297 (carve-out 4 — auto-archiving four mail classes)

4. **Auto-archiving four mail classes.** The class: an inbox thread the sweep
   labelled **Newsletters & Learning** or **Promotions & Ads**; or **Receipts
   & Subscriptions** after its ledger row was written and read back; or
   **Schedule Calendar** after a matching event was found on either swept
   calendar, or an event was created on Eriks's per-item yes (WIDENED 2026-09-09: or under carve-out 7), or the thread was re-routed to Promotions by the daily-agenda / webinar / ticket-vendor
   sub-rule. Archive means removing `INBOX` only. Both boundaries: **not** the
   other nine classes (Needs-Payment, Reply/Do, Family & Personal, Banking &
   Cards, Professional Networking, Social Media, Loyalty, Security &
   Verification, Travel), which stay in the inbox with their label; and
   **never trash, spam or mark read** — archiving is the whole permission (NARROWED 2026-09-09: carve-out 6 trashes exactly one class, Google Calendar notifications). Authorising decision, Eriks, 2026-09-07, choosing among the options
   offered: *"Carve-out: auto-archive exactly those four classes."* Recorded
   in `logs/run-log.md` with the alternatives he declined.

## §C5 — From AGENTS.md lines 298–314 (carve-out 5 — closing the payment loop)

5. **Closing the payment loop.** The class: an inbox thread that still
   carries **Needs-Payment** and whose payment task (title `Pay …`,
   description line `ref: mail:<thread_id>`) Eriks has **completed** in
   Todoist. On it, and only on it, the run applies Eriks's own `Paid` label,
   removes `Needs-Payment`, and removes `INBOX` — the one place a label is
   ever removed and the one label written outside the thirteen. Both
   boundaries: **not** a task merely moved to Done (Done is the assistant's
   proposal column, never a completion), and **not** a completed task without
   a `ref: mail:` line (a payment tracked from elsewhere has no thread to
   touch); and it licenses no other label removal, no other archive, and
   never trash, spam or mark-read. Known cost, accepted by Eriks: a task
   completed by mistake handles its mail the same way. Authorising decision,
   Eriks, 2026-09-07, asking *"Do you have the rule in place when the
   "payment" task has been closed (DONE), change the label on the
   corresponding e-mail and archive it?"* and, on the cost being named:
   *"it's okay, I won't click by mistake. worst case, they (whoever that is)
   will send a reminder later on."* Procedure in `procedures/step-2-triage.md` § Closed payment tasks.

## §C6 — From AGENTS.md lines 315–338 (carve-out 6 — trashing Google Calendar notification mail)

6. **Trashing Google Calendar notification mail.** The class: an inbox
   thread that is Google Calendar's own notification — its body carries
   Google's notice text ("Invitation from Google Calendar" / "You are
   receiving this email because you are subscribed to Calendar
   notifications") or it is a Google Calendar daily agenda — and its subject
   begins "New event:", "Updated event:", "Cancelled event:", "Invitation:",
   "Updated invitation:", "Accepted:", "Declined:", "Tentatively accepted:"
   or "Daily agenda" (WIDENED 2026-09-21 by Eriks, answering `[Needs Eriks]`
   task `6hWmrXJ7Cg6hF9Wx` with *"a)"*, where option (a) read *"Add
   "Notification:" to carve-out 6's prefix list — reminder mails get trashed
   like the rest"*: the prefix **"Notification:"** — Google's upcoming-event
   reminder — is now in the list. Superseded default: reminders were
   labelled Schedule Calendar, matched and archived under carve-out 4). Trash means `trash_thread` only (recoverable from
   Gmail's Trash for 30 days). Before trashing a creation or invitation
   notice, the run checks the event exists on a swept calendar or has a
   cancellation in the same sweep; if neither, carve-out 7 runs first, then
   the trash. Both boundaries: **not** a booking or invitation from any
   sender other than Google Calendar's notifier (a clinic, an airline, a
   ticket vendor, a person's own `.ics`), which stays Schedule Calendar and
   goes through carve-out 7; and it licenses no trashing of any other class,
   no spam, no mark-read. Supersedes the daily-agenda half of the Schedule
   Calendar sub-rule (daily agendas were re-routed to Promotions; now they
   are trashed); the marketing-webinar and `bilesuserviss.lv` halves
   survive. Authorising decision, Eriks, 2026-09-09, in chat: *"The notifications from google should be ignored (those are usually about the event creation or deletion) so those can be deleted immediatlly."*

## §C7a — From AGENTS.md lines 339–357 (carve-out 7 — base rule)

7. **Creating the event for an unmatched invite.** The class: an inbox
   thread the taxonomy labels Schedule Calendar as a real invitation,
   booking confirmation or itinerary (not carve-out 6's Google notices, not
   marketing) whose event — title roughly equal, times overlapping — exists
   on neither swept calendar and which carries no cancellation. On it the
   run calls `create_event` on Eriks's own calendar
   (`epetersons87@gmail.com`) with the title, start, end (60 minutes when
   the mail gives none) and location from the mail, **no attendees**, and a
   description carrying `ref: mail:<thread_id>`; reads it back; logs it with
   the event id; then archives the thread under carve-out 4. Both
   boundaries: **not** the Family calendar or any other calendar (the write
   surface stays Eriks's own), never attendees (an invite reaches another
   person), never `update_event`, `delete_event` or `respond_to_event`; and
   **not** an advertising event (webinar, Q&A, launch, summit), which still
   goes to Promotions. Unsure whether the mail is a real booking → a
   `[Needs Eriks]` question, default *not created*. Known cost, accepted:
   a wrong creation is a stray event on Eriks's own calendar, named in the
   brief. Supersedes "a calendar proposal in the brief, created on a
   per-item yes". Authorising decision, Eriks, 2026-09-09, in chat: *"Same goes with calendar invites - check if those exist, if not, create one."*

## §C7b — From AGENTS.md lines 358–372 (carve-out 7 — NARROWED 2026-09-15, past-dated bookings)



   **NARROWED 2026-09-15 by Eriks (applied 2026-09-17) — a booking whose
   date has already passed is never created, and its thread is archived
   anyway.** The appointment is over, so it counts as *handled* for
   carve-out 4's purposes even though nothing was written to the calendar.
   This closes the gap the 2026-09-14 Tesla test-drive case opened, where
   the letter of the trigger (real, uncancelled, on neither calendar) was
   satisfied but the purpose of the permission — not missing a **future**
   commitment — was defeated. Authorising decision, Eriks, 2026-09-15,
   answering `[Needs Eriks]` task `6hW4pChWvwJ2jp5Q` with *"a)"*, where
   option (a) read *"Never create a past-dated event, and **do** archive the
   thread anyway once the date has passed — the appointment is over, so
   treat it as handled."* Superseded default: *not created, not archived.*


## §C7c — From AGENTS.md lines 373–387 (carve-out 7 — WIDENED 2026-09-15, Travel)

   **WIDENED 2026-09-15 by Eriks (applied 2026-09-17) — the class now
   includes Travel - Bookings & Iterinary.** A confirmed, **future**,
   uncancelled hotel, flight or car booking labelled Travel gets the same
   treatment as a Schedule Calendar invite: `create_event` on Eriks's own
   calendar, no attendees, `ref: mail:<thread_id>` in the description, read
   back and logged. The thread keeps its **Travel** label and is **not**
   archived by this — carve-out 4's archivable classes are unchanged.
   Authorising decision, Eriks, 2026-09-15, answering `[Needs Eriks]` task
   `6hW4pGFRrff5M2FQ` with *"b)"*, where option (b) read *"Extend carve-out
   7 to Travel: a confirmed, future, uncancelled hotel/flight/car booking
   gets an event on your own calendar (no attendees, `ref: mail:` in the
   description), read back and logged."* This also settles the 2026-09-14
   Travel-vs-Schedule tie-break defect: the two classes no longer differ in
   what they cause the run to write, so the label choice is filing again.


## §C7d — From AGENTS.md lines 388–399 (carve-out 7 — ANSWERED 2026-09-21, multi-day stays)

   **ANSWERED 2026-09-21 by Eriks, in the task's comments:** *"Seperate
   check-in / check-out events + full day for the stay."* A multi-day stay
   now creates **three** events on Eriks's own calendar: one all-day span
   over the stay (arrival date to departure date inclusive), a timed
   check-in entry on the arrival day (the mail's stated check-in time, else
   a 60-minute default at 16:00) and a timed check-out entry on the
   departure day (ending at the mail's stated check-out deadline, else a
   60-minute default ending 11:00). No attendees, `ref: mail:<thread_id>` on
   each, each read back and logged. The Hotel Fisserhof stay, 3–10 Jan 2027,
   was created this way the same day. Superseded default: *a multi-day stay
   creates nothing.* The paragraph below is kept as the record of the gap.


## §C7e — From AGENTS.md lines 400–416 (carve-out 7 — DECIDED 2026-09-30, overlapping differently-titled event)

   **DECIDED 2026-09-30 by Eriks — an overlapping event with a different
   title is not a match, so a flight still gets its own timed event.**
   Answering `[Needs Eriks]` task `6hc967pfqvR7J3qQ` with *"a)"*, where
   option (a) read *"Keep the default — flights get their own timed events
   regardless of what else overlaps."* The match test stays exactly as
   written — **title roughly equal AND times overlapping** — so a
   trip-level entry on either swept calendar does **not** suppress a booking
   event that overlaps it; the reference case is the all-day **"Viena"** span
   of 23–25 Sep on the Family calendar, which did not stop the two airBaltic
   BT273/BT274 flight events being created. Declined: (b) treating any
   overlapping event as a match whatever its title, and (c) asking per item.
   The cost Eriks accepted: a duplicate-looking event he deletes himself,
   since `delete_event` is forbidden to the assistant in every phase.
   Superseded status — this was the *stated default* of an open question and
   is now a **decision**, so a run no longer raises a `[Needs Eriks]`
   question when an overlapping differently-titled event exists.


## §C7f — From AGENTS.md lines 417–425 (carve-out 7 — WAS OPEN (superseded by 2026-09-21 answer))

   **WAS OPEN, and blocked the multi-day half of that widening:** option
   (b) also asked *"Say whether you want one all-day span for a stay (3–10
   Jan) or separate check-in/check-out entries."* Eriks did not say, and
   silence is never an answer. **Until he answers, a multi-day stay creates
   nothing** and is named in the brief; a booking with a single stated start
   time (a flight departure, a car pick-up) is unaffected and is created as
   a normal timed event. Tracked on task `6hW4pGFRrff5M2FQ`, which stays
   open with the question narrowed to that one point.


## §NE — From AGENTS.md lines 508–518 ([Needs Eriks] — Placement)

**Placement — NARROWED-IN-PRACTICE 2026-09-14, Eriks, in chat:** *"moved
proposed things to this week"*. Both open questions were moved to **This
Week**. Recorded as a **per-item placement, not a new creation rule**: new
`[Needs Eriks]` tasks are still created in Waiting / Blocked, with no due
date, until Eriks says otherwise — where they should be *created* is an open
question with him. The board layout is his to manage
(`procedures/step-2-triage.md` § 4), so a move he asks for is executed and
**never reverted by a later run**. Note the tracker showed no move of his own
when he said this: the activity log for project Personal carried no `updated`
event, so the move was performed by the assistant on his instruction.


## §R — From AGENTS.md lines 577–622 (The routine — Delivery and the chain of schedule changes)

**Delivery.** The Personal brief is the closing message in chat and is
archived to `briefs/YYYY-MM-DD.md`. The routine runs when Eriks asks
(`/start-day`); `/inbox` runs Step 1 alone; nothing is scheduled. It is
idempotent: a second run the same day no-ops. **The weekly digest** of
newsletters and promotions (`procedures/digest.md`) is an HTML slide deck in
`briefs/`, shown in chat, produced **only by `/digest`** — never by
`/start-day`, never sent anywhere. NARROWED 2026-09-07 by Eriks after the
first `/start-day` produced it: *"Digest should be just sent once a week or
on demand."* Superseded text: "produced by `/digest` or by the first run on
or after Friday 16:00". The on-demand half survives; the Friday hand-off
inside the daily routine is withdrawn. A fixed weekly trigger, if Eriks
wants one, is a scheduled `/digest`, set up only on their yes.

**ADDED 2026-09-19 — the routine also runs unattended, as a Claude Code
cloud routine.** Eriks, in chat, choosing option 1 of ten offered: *"let's
go with the option 1. Make the whole step by step plan and let's implement
it."* The instance is mirrored to the private repository
`erjaaaaaa/my-ai-os-assistant`; the routine clones it daily at 04:00 UTC
and runs the same skill under `procedures/cloud-run.md`, which is read at
Step 0 of every unattended run. The brief is delivered by commit to
`briefs/` and pushed — no message on any channel. Superseded text above:
"nothing is scheduled" — the cloud routine is scheduled; the laptop stays
on demand. `/digest` is unchanged and stays on demand. The pushed
repository is the source of truth for state from this date; every run,
laptop or cloud, pulls at Step 0 and pushes at Step 5.
**CHANGED 2026-09-20 by Eriks:** the daily routine now fires at **06:00
UTC** (09:00 Riga in summer) — *"The daily brief can then be delivered once
a day at 9 am (moved from 7 am right now)"* — and a **second cloud routine
runs `/inbox` every hour** except 06:00 UTC — *"I want the e-mail sweep
labelling to run every hour."* Superseded text: "daily at 04:00 UTC". The
hourly form is `procedures/cloud-run.md` § 4.
**CHANGED AGAIN 2026-09-20 by Eriks, later the same day:** *"Let there be
inbox sweeps every hour, but remove the daily brief from the cloud run. I
will just run /start-day myself here on the laptop whenever I'm ready."*
The daily cloud routine is **disabled** (kept, not deleted); the hourly
`/inbox` sweep runs every hour with no exception; `/start-day`, and with it
Steps 2–4 and the brief, is a laptop, on-demand command again. Also
decided: *"Ok, let's keep the vault local."* — Step 3 stays on the laptop
and the vault-online design is parked in `plans/2026-09-19-cloud-routine.md`.
**CHANGED 2026-11-01 by Eriks, in chat, after a usage review:** the sweep
now runs **every 4 hours** (`59 */4 * * *` UTC: 00:59, 04:59, 08:59, 12:59,
16:59, 20:59) on `claude-sonnet-5-5` — *"Run every 4 hours"*. Superseded
text: "the hourly `/inbox` sweep runs every hour with no exception". The
routine keeps its name and the word "hourly" in older entries; read it as
the sweep routine. A sweep now covers up to four hours of mail, not one.


## §L — From AGENTS.md lines 641–693 (The learning loop (current mechanism) — replaced in the slim file by the PROPOSED section)

### The learning loop

**`lessons-learned.md`** is read at the **start of every run** and every rule
in it is applied. When something goes wrong, or Eriks corrects the
assistant's behaviour, append one entry:

```
- YYYY-MM-DD — **Short title** — What happened. **Rule:** what to do differently.
```

**Keep rules concrete and testable.** "Be more careful" is not a rule; "never
create a task from a thread where Eriks is only CC'd" is. Never reword or
delete an existing entry — if a rule changes, append a new one saying which it
replaces.

**`lessons-learned-archive.md`** holds the full verbatim text of every
promoted entry, in chronological order. **It is not read at startup** — that
is the whole point of the split. Grep it when you need the *why* behind a rule
found in a governing file, or to audit whether a promotion was faithful.

**`config/methods.md`** holds one-line distilled *working-method* rules —
verification habits, defaults, when to ask — whose only home would otherwise
be an entry's general-rule paragraph. It **is** read at startup, so keep it
terse: one bullet, about two lines, citing its archived entry. Anything longer
belongs in a procedure file.

**The promotion review** runs periodically over every entry **older than 7
days**, and does exactly one of four things per entry:

- **verify-and-archive** — the rule is demonstrably already in a governing
  file, with the evidence named. "Probably covered" is not evidence.
- **promote-and-archive** — a clear, dated Eriks decision: consolidate it
  verbatim into the governing file, verify the edit by reading it back, then
  move.
- **distill-and-archive** — a method rule with no procedural home: one bullet
  in `config/methods.md`, then move.
- **ask the owner** — promotion needs interpretation. The entry stays until
  answered, as a `[Needs Eriks]` task.

**Entries younger than 7 days always stay**, as does anything whose promotion
needs interpretation. **An entry moves only when nothing in it would be lost
from the read path.** Moves are exact line ranges, verified verbatim after
writing; entries are **never reworded in either file.**

**Log every review run — including a zero-work one** — with counts and,
critically, the **NET read-path delta: the sum across every file the routine
opens at startup, not just this file's shrinkage.** Promotion **relocates and
compresses roughly 10:1 rather than removing**, and the file method rules are
distilled into is on the read path too. A review that reports only the hot
file's reduction is measuring itself with the one number guaranteed to
flatter it.

All paths are relative to this folder.
