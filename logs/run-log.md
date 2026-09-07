# Run log — Personal Assistant

Append-only. Newest entry at the bottom. Narrative lives here; ids and
watermarks live in `state/state.json`.

## 2026-09-07 — Setup

Instance built. Owner: Eriks. Tracker: Todoist project **Personal** — already
existed with six sections (Backlog, This Month, This Week, In Progress,
Waiting / Blocked, Done); the account already carried `someday`,
`agent-waiting` and the six `size/*` labels. **Nothing was created in any
external system during setup.** Every id was read back live and written to
`state/state.json` with `_verified: 2026-09-07`.

Census at setup: project Personal holds 23 open tasks (20 top-level, 3
subtasks under one parent); no per-section count exceeds that total.

Modules in: phase-gates, delegation-model, agent-questions, kanban-board,
sizing-and-capacity, learning-loop, knowledge-vault, voice-style-guide.

### Connection plan

**Works now** — all three control queries came back populated on 2026-09-07:

- **Todoist** — `user-info` returned the owner's own account
  (`epetersons87@gmail.com`, Europe/Riga, Todoist Pro); `find-projects`
  returned Personal, 1 of 1. Sections and labels read back.
- **Gmail** — `list_labels` returned 55 labels (9 system, 46 user);
  `INBOX` reports 24 threads. The mailbox is the personal one. Read-only in
  this build.
- **Google Calendar** — `list_calendars` returned 8 calendars; both swept
  calendars (`epetersons87@gmail.com` and Family) are in the list.

**To connect, in priority order** — nothing outstanding for the chosen
sources.

**Deferred by Eriks — recorded so it is not lost; not an outage, not a
defect:**

1. **Gmail inbox labelling sweep — off.** Eriks, 2026-09-07: "Record this to
   do later. I'll send you my setup … and I want you to replicate this setup.
   Let's work on this separately. Once this whole assistant project is set,
   then, as a separate thing, let's set up the correct labeling for personal.
   It will be different than the one I have at … the work one." Setup found
   the mailbox already carries a manual label tree (see
   `config/routing-rules.md` § Source-specific notes). Any future assistant
   namespace must be additive to that tree and must never touch it.
2. **WhatsApp — wanted, not built.** The kit has no adapter and no connector
   exists; personal WhatsApp has no official read API. Eriks, 2026-09-07:
   "Note it down, and let's explore the options. For example, I use Rambox
   client. Maybe we can connect to it somehow and read it there, or maybe
   there are some other alternatives. Let's explore, but you can leave it out
   for now, but record it to connect for later." Follow-up, as its own piece
   of work: explore read paths (Rambox, an export, a local-script adapter
   built on the read-only model where the write capability is absent rather
   than forbidden) and weigh the terms-of-service and ban risk before
   building anything.

**Needs a one-time setup only Eriks can do** — none.

**Broken rather than missing** — none.

### Rehearsal

See the entry below this one.

## 2026-09-07 — Rehearsal (read-only; nothing written to any external system)

Watermarks at zero, sweep bounded to the last three days.

**Heartbeat.** Gmail: live — `list_labels` populated (55 labels);
`INBOX.threadsTotal` 24; 17 inbox threads read (17 ≤ 24). Calendar: live —
`list_calendars` populated (8), both swept ids present. Todoist: live —
`user-info` returned the owner's account; project Personal 23 open tasks.

**Mail, inbox, 3 days: 17 threads.** Act 1 (Bite Latvija invoice, 20.06 EUR,
due 20.09.2026 — Eriks's own label `Action Required/Needs-Payment` on it);
Know 3 (Etsy payout notice; a genealogy-site birthday alert; a LinkedIn
"message waiting" notification); needs-owner 1 (Amex statement-ready notice
— no amount or due date in the mail; question: is the card on direct debit,
or should each statement become a pay task? default: no task); Noise 12
(newsletters, promotions, social and platform notifications, loyalty-terms
update). Sent mail, 3 days: 0 threads (believable against a populated
control; `SENT` holds 1,533 threads mailbox-wide).

**Calendar, 7 days.** Own calendar: 1 event (a 01:00 online Q&A on 8 Sep;
no prep, no task). Family calendar: 8 events — two football sessions, a
chess session on 11 Sep, three rubbish-collection reminders, a cleaner visit,
one more football; two of them were added today by Eriks's wife. No
conflicts. No invitations awaiting response. Tasks: none — the chess session
relates to the open task "Contact in-home chess coaches" but is not evidence
of movement; flagged as possibly related, no move.

**Open questions.** `agent-waiting` in Personal: 0 (believable against 23
open tasks in the project).

**Vault.** Nothing durable in the window; no snapshot.

**Would have written (did not):** 1 task (Bite invoice, Backlog, p3,
`size/S`, ref `mail:1a072afdab0b973b`); 1 `[Needs Eriks]` task (Amex
statements, Waiting / Blocked, `agent-waiting`, p4); watermark
`mail.last_internaldate_ms` → 1788789664000; `calendar.last_scanned_date` →
2026-09-07. All left unwritten; state unchanged.

**Could not decide:** whether a card statement notice with no due date is a
payment obligation — recorded above as the needs-owner item rather than
guessed.

## 2026-09-07 — Inbox labelling built (replication of the earlier automation)

Source: Eriks's exported "Gmail labeling agent" workflow (96 nodes), read in
full. Replicated: the thirteen-class taxonomy onto Eriks's own labels (ids
read back from `list_labels` and written to state), Needs-Payment → Todoist
payment task (replacing the messaging-app notice, by Eriks's request),
Schedule Calendar handling with the match check on both calendars, receipts
extraction from body text, newsletter and promotion ledgers, and the weekly
digest. New files: `procedures/step-1-inbox.md`, `procedures/digest.md`,
`ledgers/README.md`, skills `/inbox` and `/digest`. The routine was renumbered
to the kit's canonical order (Step 1 inbox, 2 triage, 3 ingest, 4 brief,
5 close out).

**Eriks's decisions, in his words (2026-09-07):**
- Archiving — offered: label only; carve-out for Newsletters + Promotions only;
  carve-out for the four classes the automation archived. Chosen:
  *"Carve-out: auto-archive exactly those four classes."* Recorded as
  carve-out 4 in `AGENTS.md` § Phase gates.
- Calendar events from Schedule mail — offered: propose and create on per-item
  yes; auto-create solo events; never create. Chosen: *"Propose in the brief;
  create only on my per-item yes."* Attendee invites are never created.
- Ledgers — chosen: *"Local CSV files in the instance, committed to git."*
  Then: *"Before writing into CSVs … I'd like to migrate the data from the
  existing google sheets so we don't lose the historical data and can analyse
  it later."* → ledgers are not created until migrated (`ledgers/README.md`).
- Digest — chosen: *"Deliver as HTML slide (using /frontend-slides skill)
  summary in the chat."*
- Trigger — chosen: *"Part of /start-day, plus /inbox on demand."*
- Cutover — chosen: *"I'll deactivate n8n after the rehearsal."*

**Capability findings:** the Gmail connector cannot download attachments (no
PDF fallback); the Drive connector is signed in as the work account and cannot
see the personal sheets, and cannot append rows to any sheet; the Chrome
extension was not connected, so the sheets could not be read that way either.
Migration therefore waits on Eriks's CSV exports into `ledgers/import/`.

## 2026-09-07 — Rehearsal of Step 1 (read-only; nothing written to Gmail, Todoist or the ledgers)

**Control.** `list_labels` populated (55). `INBOX.threadsTotal` was 24 at
Step 0 (16:49); the sweep at 18:05 returned 22 threads (22 ≤ 24; two threads
left the inbox in between). Sent-mail check not part of this step.

**Already labelled, skipped: 18 of 22** — the earlier automation labelled the
inbox right up to 14:48 today. Consequence worth Eriks's eye: those 18 include
threads in the archive classes (a Newsletters thread and a Promotions thread)
that the earlier automation labelled but did not archive; under the skip rule
this instance never touches an already-labelled thread, so they stay in the
inbox until Eriks archives them or asks for a one-time pass over
already-labelled threads in the four classes. Not done; not decided.

**Unlabelled: 4.** What the sweep would have done:

| Thread | Would label | Then | Note |
|---|---|---|---|
| Decathlon, "Krājiet punktus, lai saņemtu 3 € kuponu" (7 Sep) | Loyalty | stays in inbox | Borderline: the class description says airline/hotel programmes, but the Rimi loyalty mail carries Loyalty already, and the tie-break is Loyalty > Promotions. Worth widening the description to "any loyalty programme" — Eriks's call. |
| Printful, "Price changes are live" (27 Aug) | Reply/Do | stays in inbox; Step 2 would task "Review Printful price changes and adjust store prices" (p4, size/M) | The sibling thread "Price changes coming August 27" already carries Reply/Do. |
| Whoop, "Introducing Meridian" (27 Aug) | Promotions & Ads | ledger row + archive | **Blocked: ledger not migrated.** Would be labelled only, and reported as waiting. |
| Swedbank tool, "Tavi dokumenti SIA reģistrācijai ir gatavi!" (23 Aug, starred by Eriks) | none — listed | — | A system notice carrying an action (register the company within 14 days; the window closed 6 Sep). Unsure between Reply/Do and Family & Personal, so listed rather than guessed. |

**Payment tasks:** none new — the Bite invoice thread already carries
Needs-Payment (labelled by the earlier automation), so Step 1 skips it; Step 2
triage still creates its task by ref (see the earlier rehearsal). **Calendar
proposals:** none. **Receipts logged:** none. **Review task** that would be
upserted: `[Act] Review inbox labels — 2026-09-07`.

**Migration status:** the three ledgers do not exist yet. The Drive connector
is signed in as the work account and cannot see the personal sheets; the Chrome
extension was not connected. Waiting on Eriks's CSV exports into
`ledgers/import/` (see `ledgers/README.md`).
