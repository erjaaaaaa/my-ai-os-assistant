# Routing rules — how an item becomes a task, or does not

Read at Step 0. Applied at Step 2 (triage). This file is policy; the steps
that apply it live in `procedures/step-2-triage.md`. When the two disagree,
this file wins and the disagreement is a defect to fix.

**Standing bias: when in doubt, do not create a task.** A missed task costs
Eriks one glance at the brief; a wrong task costs trust in the whole system.

**Eriks's standing decision on scope (2026-09-07, their words):** "for my
personal project, I don't want any exclusions. I want medical appointments,
financial, and whatever admin obligations to be all created as tasks because
these are my personal obligations, and I need to find time and plan them."
So a doctor's appointment, a bill, a tax deadline, a school form or an
insurance renewal is an ordinary task here, classified on content like
anything else. There is no sensitive-content filter in this instance.

## Ownership tests — applied before bucketing

1. **Addressed directly, with an ask** → Act or Decide. **Copied only, or one
   of many recipients with no ask to Eriks** → Know.
2. **Eriks's own "I'll do X"** — in a sent message — is the highest-signal
   item in any sweep. Attribution is not in doubt.
3. **Does this ask Eriks to *do* something, or to *attend* something?** Attend
   is calendar, not task.
4. **Is this the platform's machine-generated notification, or a person's
   message?** A tool naming Eriks as the only person who can act is evidence
   about that tool's permission model, not about who owns the decision. A
   named human asking is ordinary mail.
5. **A name is a claim.** A first name plus context is a hypothesis. Write
   "the message says X; likely Y, unconfirmed".

## The six buckets

| Bucket | Test | Output |
|---|---|---|
| **Act** | Eriks must do a concrete thing, and the source says so or Eriks said so themselves. | Task in Personal, Backlog section, imperative title, priority, `ref:` line. |
| **Decide** | A choice only Eriks can make, with options visible in the source. | Task titled `Decide: …`, options in the description, no due date unless the source states one. |
| **Delegate** | Someone else's work; only the handoff is Eriks's. | Task titled `Delegate: … → <person>`; no nameable person → `[Needs Eriks]` instead. |
| **Know** | Context Eriks should have; nothing to do. | One line in the brief. A durable item (a decision, a person, a document) is a vault-snapshot candidate — see `procedures/step-3-ingest.md`. |
| **Noise** | Promotions, newsletters, automated receipts with no action, platform notifications, social chatter. | Nothing. Counted in the run log. |
| **needs-owner** | Classification confidence is low, or two rules disagree. | `[Needs Eriks]` task, label `agent-waiting`, section Waiting / Blocked, default stated. Never a guess. |

Priority mapping: a stated deadline within 7 days or money at risk → p2; a
stated deadline beyond that or a named person waiting → p3; everything else
p4. p1 is never set by triage.

Due dates: only when the source states one in the sender's own words. A
relative time word in a quoted or forwarded message ("Friday", "next week")
cannot be resolved and gets no due date.

## Anti-duplication

Every task the assistant creates carries `ref: <reference>` as the first line
of its description — `mail:<thread_id>` or `event:<calendar_id>/<event_id>`.
Before creating, check the ref against **open** tasks in Personal, **completed**
tasks in the last 60 days, and **deleted** tasks in the last 60 days (via the
activity log). A match is an update to the existing task, never a second task.
One thread produces at most one task; a thread with three asks produces one
task with three lines.

Never relabel, retitle or move an existing task just to justify creating
another.

## Source hints

### Mail

- **A thread where Eriks sent the last message is usually waiting on someone
  else, not on Eriks.** Do not task it unless a follow-up date is stated.
- **A direct question addressed to Eriks by a named human is the strongest
  signal this source produces.**
- **Machine-generated calendar notification mail is disposable** — the
  calendar holds the event. A person writing prose about an appointment is
  not a notification and is classified on content.
- **Automated platform notifications asking Eriks to approve, verify or
  confirm because they are the registered account holder** are Noise unless
  they carry a genuine account-owner decision — a bill due, a security
  incident, an ownership change — in which case they are Act.
- **Receipts, subscriptions and bank notices** are Know unless a payment is
  due, a renewal needs a decision, or a charge looks wrong — then Act or
  Decide.
- **Ignore read/unread state entirely.** In a mailbox that is unread by
  default, read state carries no signal.

### Calendar

- **An appointment needing preparation becomes a task only if the preparation
  is non-trivial** — a document to bring, a form to complete, a number to look
  up, a decision to pre-form. "Attend" is not a task.
- **Routine recurring series generate no tasks.** None are named yet; name
  them under Source-specific notes when they appear.
- **Only an event with a real person on the other side is an appointment.**
  ADDED 2026-09-08 by Eriks: *"For calendar events / invites. I want only
  real people ones. For example one with Nate is the advertising. You can
  ask in such cases to be sure."* Test: a named human counterpart — a
  doctor, a coach, a friend, a family member, a named colleague — or an entry
  someone put on the Family calendar → **real**. A session a company or a
  creator promotes to an audience — a webinar, Q&A, live session, launch,
  summit, masterclass — is **advertising**, however it reached the calendar
  (self-created after a registration included; "Q&A w/ Nate" on 8 Sep 2026
  is the reference case). Advertising is not listed under the brief's "Today
  and the next 7 days", gets no prep flag and no task; it is counted in the
  brief's Anomalies as "advertising events on your calendar: N". **Unsure →
  ask**, one `[Needs Eriks]` question per event, deduped on
  `event:<calendar_id>/<event_id>`, default *advertising — not listed*. The
  same test decides the mail side: a Google Calendar notification or reminder
  for an advertising event follows the Schedule Calendar sub-rule to
  Promotions & Ads.
- **Unanswered invitations produce no task.** Name them in the brief's
  calendar section and stop.
- **A conflict is worth surfacing even though it is not a task** — including
  a conflict between the two swept calendars.
- **A Family-calendar event is Eriks's to know about, not necessarily to act
  on.** It is listed in the brief's Family section; it becomes a task only when
  its title or description carries a concrete ask for Eriks or non-trivial
  preparation.
- **An event on any calendar not in scope is never Eriks's commitment**, even
  when it names them.

## Source-specific notes — the tunable scope

### Mail scope

- Mailbox: `epetersons87@gmail.com` only.
- Sweep: the whole inbox each run (it held 24 threads at setup; add a per-run
  cap here, with a "remainder reported, not skipped" rule, if it grows past
  ~200), plus sent mail since the watermark for Eriks's own commitments.
- Excluded wholesale: nothing, by Eriks's decision above.
- **The label sweep (Step 1) writes into Eriks's own label tree** — the
  thirteen classes in § Mail label taxonomy, one per thread, ids in state. A
  thread already carrying any of the thirteen is **skipped**: whether Eriks or
  an earlier run put it there, that label wins. Labels are only ever **added**;
  never removed, never renamed, never created, and never any label outside the
  thirteen.
- **Eriks's own label tree — never removed, never renamed, never created:**
  `Action Required` (with `Reply/Do`, `Schedule Calendar`, `Needs-Payment`,
  `Waiting / Follow-up`), `Finance & Accounts` (with `Banking & Cards`,
  `Paid`, `Receipts & Subscriptions`), `Interests & Marketing` (with
  `Professional Networking`, `Promotions & Ads`, `Newsletters & Learning`,
  `Social Media`), `Archive-Categories/*`, `Family & Personal`, `Security &
  Verification`, `Loyalty`, `Travel - Bookings & Iterinary`, `Notes`. The
  thirteen taxonomy classes below are the only ones the assistant applies; the
  rest (`Archive-Categories/*`, `Paid`, `Notes`, `Waiting / Follow-up`) are
  Eriks's alone and are read as signal only.

### Calendar scope

- In scope: `epetersons87@gmail.com` and the Family calendar (ids in state).
- Window: today plus 7 days, Europe/Riga.
- Ignored: the two calendars named "Todoist" (task mirrors — sweeping them
  would count every task twice), the three holiday calendars, and any event
  calendar. Add a calendar here only when Eriks names it.
- Routine recurring series: none named yet.

## Mail label taxonomy — the thirteen classes and what follows each

Applied by `procedures/step-1-inbox.md`. Every inbox thread that carries none
of the thirteen gets **exactly one**, chosen from the full thread text (not the
snippet). The class then decides what else happens. Carried over in substance
from Eriks's earlier automation on 2026-09-07.

| Class (Eriks's label) | Test | Then |
|---|---|---|
| **Needs-Payment** (`Action Required/Needs-Payment`) | A request to pay a specific invoice or bill, or approve a payment due: invoice/receipt numbers, due dates, amounts, bank details, "pay invoice", "payment due", "outstanding balance". **Not** fundraising or charity ("donate", "give now", "support our mission"), marketing, newsletters, or anything with an unsubscribe cue. | Label + **payment task** (shape below). Stays in inbox. |
| **Reply/Do** (`Action Required/Reply/Do`) | A direct ask needing Eriks's reply or a small action: "Can you confirm?", "Please send me the file", "Can you do X?" | Label. Feeds Step 2 triage (Act / Decide). Stays in inbox. |
| **Schedule Calendar** (`Action Required/Schedule Calendar`) | **Only** real invites, updates or confirmed bookings: must contain invitation / accepted / declined / rescheduled / canceled, or an `.ics`, or an explicit travel booking (itinerary, boarding pass). Newsletter cues (unsubscribe, manage preferences) mean it is **not** Schedule. | Sub-rule first: a Google Calendar daily agenda or "no events scheduled" mail, a marketing webinar / live session / register / sign up / subscribe / YouTube event, or anything from `bilesuserviss.lv` → label **Promotions & Ads** instead and archive (WIDENED 2026-09-08: also a Google Calendar notification or reminder for an event that § Source hints › Calendar classes as advertising; unsure → ask, per that rule). Otherwise: label, then the calendar match check and, if unmatched, a **calendar proposal** in the brief. Archived once handled. |
| **Family & Personal** | Personal or family messages: school, kids, health, family updates. | Label. Stays in inbox. |
| **Banking & Cards** (`Finance & Accounts/Banking & Cards`) | Bank statements, card alerts, suspicious transactions, balance notifications. | Label. Stays in inbox. |
| **Receipts & Subscriptions** (`Finance & Accounts/…`) | Bills already charged, invoices paid, subscriptions, renewals, receipts showing charges. | Label + **ledger row** in `ledgers/receipts.csv`, then archive. |
| **Newsletters & Learning** (`Interests & Marketing/…`) | Recurring editorial content from publishers (Substack, Beehiiv, Mailchimp, industry blogs). Google Calendar daily agendas land here too. | Label + row in `ledgers/newsletters.csv`, then archive. Digested weekly. |
| **Professional Networking** (`Interests & Marketing/…`) | LinkedIn, AngelList and other professional-community notifications. | Label. Stays in inbox. |
| **Promotions & Ads** (`Interests & Marketing/…`) | Marketing, retail offers, casino ads, sales campaigns, discounts. Includes fundraising appeals and NGO campaigns unless a specific bill is due. | Label + row in `ledgers/promotions.csv`, then archive. Digested weekly. |
| **Social Media** (`Interests & Marketing/…`) | Facebook, Instagram, TikTok, X and similar notifications. | Label. Stays in inbox. |
| **Loyalty** | Airline or hotel loyalty programmes: miles, points, tier status, bonus offers, statements. | Label. Stays in inbox. |
| **Security & Verification** | Verification codes, login alerts, password resets, suspicious sign-in notices. | Label. Stays in inbox. |
| **Travel - Bookings & Iterinary** | Flight, hotel or car bookings with itineraries, boarding passes, check-in mail, travel confirmations. | Label. Stays in inbox. |

**Tie-breaks.** If a thread fits several, pick the most specific: **Travel >
Loyalty > Receipts > Promotions.** Unsure between Newsletters and Promotions →
**Promotions**. Unsure between any action class (Needs-Payment, Reply/Do,
Schedule) and anything else → leave the thread **unlabelled and list it in the
brief** rather than guess; a wrong action label costs more than a missing one.

**The payment task** (Needs-Payment only): title `Pay <vendor> <amount>
<currency>` (omit what the mail does not state; never invent an amount from an
attachment the connector cannot read); **This Week** — CORRECTED 2026-09-07
by Eriks: *"For any e-mail that are required payments - always put in THIS
WEEK column in todoist."* Superseded text: "Backlog". This is the one task
class that does not land in Backlog; every other task created by a run still
does. NARROWED 2026-09-08 by Eriks: the daily review task (`[Act] Review inbox
labels — YYYY-MM-DD`, `procedures/step-1-inbox.md` § 5) is the second class
that lands in This Week — *"When you add review labels tasks, don't just mark
it for today, but also move it THIS WEEK column, same as with the payments."*
Every other task class still lands in Backlog. Section id `this_week` from `state/state.json`; `dueString` = the due date in
the sender's own words, else none; p2 if due within 7 days, else p3; `size/S`;
description: `ref: mail:<thread_id>`, `source: <sender>, <date>`, then amount,
due date, invoice or client number, and the thread link. Dedupe on the ref
before creating. The thread keeps its label and stays in the inbox until Eriks
pays and archives it. WIDENED 2026-09-07 by Eriks: "until Eriks pays and
archives it" becomes "until Eriks **completes the task**" — the run then
applies `Paid` (id `paid_label_id` in state), removes `Needs-Payment`, and
archives the thread. Eriks: *"when the "payment" task has been closed (DONE),
change the label on the corresponding e-mail and archive it"*. Carve-out 5 in
`AGENTS.md` § Phase gates; steps in `procedures/step-2-triage.md` § Closed
payment tasks. Idempotent by thread state: a thread already carrying `Paid`
and not `Needs-Payment` is never touched again.

**The auto-archive classes** — Newsletters & Learning, Promotions & Ads,
Receipts & Subscriptions (after the ledger row is written and verified), and
Schedule Calendar (after a calendar match, an event created on Eriks's yes, or
the sub-rule re-route) — are archived by removing `INBOX` from the thread.
This is the carve-out recorded in `AGENTS.md` § Phase gates; no other class is
ever archived, and nothing is ever trashed.
