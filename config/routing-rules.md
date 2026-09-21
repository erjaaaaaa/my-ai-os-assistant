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
- **A card "statement ready" notice carries no task.** ADDED 2026-09-09 by
  Eriks, answering the `[Needs Eriks]` question on the American Express
  "Your latest statement is ready" mail (ref mail:1a076a154d35a998): *"No
  task, this is just notification. I don't use it for long time, but it just
  keeps coming since the card itself is still active."* A statement notice
  from any card issuer stays Know-only (Banking & Cards label, at most one
  line in the brief); a task arises only when the mail itself states an
  amount due or a due date, which then makes it Needs-Payment.

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
- Routine recurring series — ADDED 2026-09-09 by the assistant, per § Source
  hints › Calendar ("name them under Source-specific notes when they
  appear"); Eriks may strike any. Seen recurring on the Family calendar:
  🗑️ Sadzīves atkritumi (all-day, Wednesdays), Ljuba uborka (Thu 09:00),
  Take out rubbish (Thu 22:00), Мусор (Fri 08:00), Ervins Futbols (Thu 17:00
  and Tue 18:00, Mārupes Valsts ģimnāzijas stadions). Seen recurring on
  Eriks's own calendar: Top up HSBC (all-day, seen 29 Aug and 29 Sep), Unsub
  session (Fri 16:00, seen 4 Sep and 2 Oct). None generates a task; Family
  ones are still listed in the brief's Family section. WIDENED 2026-09-11 by
  the assistant, under the same rule ("name them when they appear"): two more
  Family all-day waste series seen in the 11–18 Sep window — 🍾 Stikla
  iepakojums (Thursdays, every 4 weeks) and ♻️ Šķirotie atkritumi (Thursdays,
  every 2 weeks). Same treatment: no task, still listed in the Family section.
  Eriks may strike either. WIDENED 2026-09-14 by the assistant, under the same
  rule: **Mark psiholog** (Family, Mondays 16:00–17:00, created 2026-09-10 by
  margaritaeliya, occurrences seen 14, 21 and 28 Sep and 5 Oct) — a child's
  recurring appointment. Same treatment: no task, listed in the Family section.
  Eriks may strike it. WIDENED 2026-09-21 by the assistant, under the same rule: **Снять
  счетчики** (Family, monthly, created 2024-05-30 by margaritaeliya,
  occurrence seen Mon 28 Sep 20:15–21:15) — a household meter-reading
  chore. Same treatment: no task, listed in the Family section. Eriks may
  strike it.
- Confirmed per-item ownership on the Family calendar — ADDED 2026-09-11,
  **SETTLED 2026-09-14 by Eriks**. Superseded text: *"**These are recorded
  facts, not a rule**: no general test has been agreed, and the open question
  `[Needs Eriks]` (ref `calendar:family-event-ownership`) asks for one, with
  the default keep flagging every overlap and ask per item. Do not generalise
  from these two."* The question is now answered: Eriks chose option (a) in the
  task's comments with the single word *"a)"*, where (a) read *"Keep the
  default — keep flagging them, you answer per item."* **Rule, and it is now a
  decision rather than an unanswered default: every overlap between Eriks's own
  calendar and the Family calendar is surfaced in the brief as a possible
  conflict, and Eriks resolves it per item.** No ownership is ever inferred from
  the creator of a Family event, from its title, or from the kind of appointment
  it is — the two facts below stay per-item facts and license no general test.
  Declined explicitly: (b) treating anything created by margaritaeliya as hers
  unless it names Eriks or a child, and (c) a narrower personal-care-only
  version of the same. The cost Eriks accepted is one line in the brief on the
  days an overlap happens.
  - **A KLINIKA** (Fri 18 Sep 10:00–13:00, created by margaritaeliya) is
    Eriks's wife's. Eriks: *"A KLINIKA is my wife's appointment. USG is mine."*
    Not a conflict with the 18 Sep Transrektālā USG.
  - **Массаж** (Fri 11 Sep 10:30–11:30, created by margaritaeliya) is his
    wife's. Eriks: *"massage is wife's"*. Not a conflict with the 11 Sep USG
    vairogdziedzerim.

### Standing reminders — carried in every brief until their end date

ADDED 2026-09-08 by Eriks, in chat: *"I need to start drinking 8000DV
(vitamin D) daily for 3 months - please add it to my todoist daily and
remind me about it in the briefs."* A standing reminder is one line in the
brief's **Standing reminders** section (`procedures/step-4-brief.md` § 6a)
on every run from its start date to its end date inclusive; after the end
date the line is dropped and the entry here is marked `ENDED`. The Todoist
task is the commitment; the brief line is the reminder Eriks asked for, not
a second task. Nothing is created, completed or moved by this rule.

| Reminder | Todoist task | Start | End | Status |
|---|---|---|---|---|
| Take vitamin D (8000DV) daily | `6hRpMx6gfFHWRcfQ`, `ref: chat:2026-09-08-vitamin-d`, recurring every day ending 2026-12-08, This Week | 2026-09-08 | 2026-12-08 | active |

## Mail label taxonomy — the thirteen classes and what follows each

Applied by `procedures/step-1-inbox.md`. Every inbox thread that carries none
of the thirteen gets **exactly one**, chosen from the full thread text (not the
snippet). The class then decides what else happens. Carried over in substance
from Eriks's earlier automation on 2026-09-07.

| Class (Eriks's label) | Test | Then |
|---|---|---|
| **Needs-Payment** (`Action Required/Needs-Payment`) | A request to pay a specific invoice or bill, or approve a payment due: invoice/receipt numbers, due dates, amounts, bank details, "pay invoice", "payment due", "outstanding balance". **Not** fundraising or charity ("donate", "give now", "support our mission"), marketing, newsletters, or anything with an unsubscribe cue. WIDENED 2026-09-14 by Eriks, answering the `[Needs Eriks]` question of the same day (ref `mail:1a099e498f6dc9a7`, task `6hW4p9MqPQFRg46Q`): **a failed-payment or update-your-payment-method notice from a subscription or service is Needs-Payment**, even with no amount, invoice number or due date — the markers this test otherwise keys on. Eriks: *"Youtube premium should be marked as Needs-payment since my payment failed."* The payment task follows per § The payment task, titled from the vendor with no amount. | Label + **payment task** (shape below). Stays in inbox. |
| **Reply/Do** (`Action Required/Reply/Do`) | A direct ask needing Eriks's reply or a small action: "Can you confirm?", "Please send me the file", "Can you do X?" WIDENED 2026-09-21 by Eriks, answering `[Needs Eriks]` task `6hXM3WGgqj8QqGRQ` with *"a)"*, where option (a) read *"Reply/Do — a small decision of yours; it stays in the inbox with a label, no task"*: **a vendor account or service notice that needs no payment but carries a consequence** — a trial expiry, a policy change, a plan downgrade — is Reply/Do, no task. First items: Bluehost WHOIS-privacy expiry `mail:1a0b32766a32b7bf` (the item the options were written for) and Google AI Studio lower-tier `mail:1a0be726fb266eaf` (the second item named on the same task, labelled under the **assistant's reading** that the answer settles the class the task's last comment asked about — Eriks may narrow it to the one item). Superseded: the tie-break left these unlabelled. | Label. Feeds Step 2 triage (Act / Decide). Stays in inbox. |
| **Schedule Calendar** (`Action Required/Schedule Calendar`) | **Only** real invites, updates or confirmed bookings: must contain invitation / accepted / declined / rescheduled / canceled, or an `.ics`, or an explicit travel booking (itinerary, boarding pass). Newsletter cues (unsubscribe, manage preferences) mean it is **not** Schedule. | Sub-rule first: a Google Calendar daily agenda or "no events scheduled" mail, a marketing webinar / live session / register / sign up / subscribe / YouTube event, or anything from `bilesuserviss.lv` → label **Promotions & Ads** instead and archive (WIDENED 2026-09-08: also a Google Calendar notification or reminder for an event that § Source hints › Calendar classes as advertising; unsure → ask, per that rule). Otherwise: label, then the calendar match check and, if unmatched, a **calendar proposal** in the brief. Archived once handled. CHANGED 2026-09-09 by Eriks, two ways: (i) a **Google Calendar notification** (New/Updated/Cancelled event, Invitation, Accepted/Declined, daily agenda; WIDENED 2026-09-21 by Eriks: also "Notification:" reminders) → label, existence check, then **trash** under carve-out 6 — this supersedes the daily-agenda half of the sub-rule ("a Google Calendar daily agenda … → Promotions & Ads"); the marketing-webinar and `bilesuserviss.lv` halves survive. Eriks: *"The notifications from google should be ignored (those are usually about the event creation or deletion) so those can be deleted immediatlly."* (ii) an unmatched, uncancelled **real invite or booking** → **create the event** on Eriks's own calendar under carve-out 7, read back, then archive — supersedes "a calendar proposal in the brief"; the proposal survives only for the unsure case, as a `[Needs Eriks]` question. Eriks: *"Same goes with calendar invites - check if those exist, if not, create one."* |
| **Family & Personal** | Personal or family messages: school, kids, health, family updates. | Label. Stays in inbox. |
| **Banking & Cards** (`Finance & Accounts/Banking & Cards`) | Bank statements, card alerts, suspicious transactions, balance notifications. | Label. Stays in inbox. |
| **Receipts & Subscriptions** (`Finance & Accounts/…`) | Bills already charged, invoices paid, subscriptions, renewals, receipts showing charges. | Label + **ledger row** in `ledgers/receipts.csv`, then archive. |
| **Newsletters & Learning** (`Interests & Marketing/…`) | Recurring editorial content from publishers (Substack, Beehiiv, Mailchimp, industry blogs). Google Calendar daily agendas land here too. | Label + row in `ledgers/newsletters.csv`, then archive. Digested weekly. |
| **Professional Networking** (`Interests & Marketing/…`) | LinkedIn, AngelList and other professional-community notifications. | Label. Stays in inbox. |
| **Promotions & Ads** (`Interests & Marketing/…`) | Marketing, retail offers, casino ads, sales campaigns, discounts. Includes fundraising appeals and NGO campaigns unless a specific bill is due. | Label + row in `ledgers/promotions.csv`, then archive. Digested weekly. |
| **Social Media** (`Interests & Marketing/…`) | Facebook, Instagram, TikTok, X and similar notifications. | Label. Stays in inbox. |
| **Loyalty** | Any loyalty programme — airline, hotel or retail (Rimi, Decathlon and the like): miles, points, tier status, bonus offers, coupons, statements. WIDENED 2026-09-09 by Eriks, answering the `[Needs Eriks]` question of 2026-09-07 (ref mail:1a07c681b6c85794) with *"a)"*, where option (a) read *"Widen the description to 'any loyalty programme'"*. Superseded text: "Airline or hotel loyalty programmes: miles, points, tier status, bonus offers, statements." The tie-break Loyalty > Promotions is unchanged. | Label. Stays in inbox. |
| **Security & Verification** | Verification codes, login alerts, password resets, suspicious sign-in notices. | Label. Stays in inbox. |
| **Travel - Bookings & Iterinary** | Flight, hotel or car bookings with itineraries, boarding passes, check-in mail, travel confirmations. | Label, **then the carve-out 7 calendar check** (WIDENED 2026-09-15 by Eriks, applied 2026-09-17; superseded text: "Label. Stays in inbox."). Stays in inbox either way — Travel is **not** an archivable class. |

**Carve-out 7 now covers Travel as well as Schedule Calendar** — WIDENED
2026-09-15 by Eriks, answering the `[Needs Eriks]` question of 2026-09-14
(task `6hW4pGFRrff5M2FQ`, ref `mail:1a09bc15a1a100a3`) with *"b)"*, where
option (b) read *"Extend carve-out 7 to Travel: a confirmed, future,
uncancelled hotel/flight/car booking gets an event on your own calendar (no
attendees, `ref: mail:` in the description), read back and logged."* The
thread keeps **Travel** and stays in the inbox; only the calendar write is
added. **The multi-day form is still unanswered** — option (b) also asked
whether a stay should be one all-day span or separate check-in/check-out
entries, and Eriks did not say. Until he does: **a multi-day stay creates
nothing** and is named in the brief; a booking with one stated start time (a
flight departure, a car pick-up) is created as a normal timed event.
**ANSWERED 2026-09-21 by Eriks** (task `6hW4pGFRrff5M2FQ`): *"Seperate
check-in / check-out events + full day for the stay."* A multi-day stay now
creates three events — an all-day span over the stay, a timed check-in on
the arrival day, a timed check-out on the departure day — per `AGENTS.md`
§ Phase gates carve-out 7. The "creates nothing" default above is superseded.

**Past-dated bookings are never created but are archived anyway** — NARROWED
2026-09-15 by Eriks, answering task `6hW4pChWvwJ2jp5Q` (ref
`mail:1a09575a7a89cb22`) with *"a)"*: *"Never create a past-dated event, and
**do** archive the thread anyway once the date has passed — the appointment
is over, so treat it as handled."* Superseded default: *not created, not
archived.* Applies to a **Schedule Calendar** thread, which carve-out 4 may
archive; a Travel thread is still never archived.

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

**The two labels are mutually exclusive — ADDED 2026-09-14 by Eriks**, after a
classifier refusal left the Eco Baltia thread carrying both for several hours:
*"E-mail cannot sit at both Paid and Needs-Payment labels at the same time -
it's confusing."* **Rule: no thread ever rests carrying both `Paid` and
`Needs-Payment`.** The swap passes through that state for the duration of two
API calls and no longer. If any step of the swap fails or is refused, the
half state is a **defect to report immediately and finish as soon as Eriks
says so** — it is never left standing as an acceptable outcome, and never
carried across runs. Where this and the "stop at the refused step" rule
(`lessons-learned.md`, 2026-09-08) appear to pull against each other, both
hold: stop writing, but report the half state as needing a fix rather than as
a neutral pause.

**The Arlo per-item decision is not overturned by the widening above.** On
2026-09-10 Eriks had the `Needs-Payment` label *removed* from the Arlo
failed-payment thread with no task, because the money was already on the
account and he was waiting for the vendor to retry. That remains a per-item
instruction about one thread. It now sits visibly narrower than the class
rule, and the class rule wins by default: a failed-payment notice is labelled
Needs-Payment and gets a task unless Eriks says otherwise for that item.

**The auto-archive classes** — Newsletters & Learning, Promotions & Ads,
Receipts & Subscriptions (after the ledger row is written and verified), and
Schedule Calendar (after a calendar match, an event created on Eriks's yes, or
the sub-rule re-route) — are archived by removing `INBOX` from the thread.
This is the carve-out recorded in `AGENTS.md` § Phase gates; no other class is
ever archived, and nothing is ever trashed.
