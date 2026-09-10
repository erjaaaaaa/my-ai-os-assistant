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

## 2026-09-07 — Ledgers migrated from the Google Sheets

Drive connector reconnected to `epetersons87@gmail.com` (both sheets returned
with that owner). Method: `download_file_content` — the archive workbook
exported as .xlsx and parsed tab by tab locally; the receipts sheet exported as
CSV. The export is the whole file, so the containing total is the workbook's
own row count. `sent` mapped to `digested` (`TRUE` → `migrated-sent`, else
empty). Dedupe on `messageId`, first occurrence kept. Each ledger re-read after
writing; header, row count, first and last row confirmed.

| Ledger | Source rows | Written | Duplicates dropped | Blank messageId | sent=TRUE |
|---|---|---|---|---|---|
| newsletters | 1875 | 1875 | 0 | 0 | 1858 |
| promotions | 2544 | 2544 | 0 | 0 | 2523 |
| receipts | 533 | 525 | 8 | 0 | — |

Rows with `digested` empty are what the earlier automation had not yet sent in
a digest; the first `/digest` picks them all up. The Google Sheets were not
modified. Rows the earlier automation adds before it is deactivated can be
re-migrated by repeating this step; dedupe absorbs the overlap.

## 2026-09-07 — `/inbox` (Step 0 + Step 1 + close-out) — Gmail write scope refused; no external write landed

**Orient.** Gmail `list_labels` populated (45 labels; `INBOX.threadsTotal`
26, `messagesTotal` 27). Calendar `list_calendars` populated (8 calendars;
both swept ids present). Todoist `user-info` returned
`epetersons87@gmail.com`. Ids `_verified` 2026-09-07 — not re-resolved.
Open `[Needs Eriks]` questions in Personal: 0 open, 0 answered, 0 ambiguous.
Watermarks at start: `mail.last_internaldate_ms` 0, `inbox.last_sweep_date`
null, `calendar.last_scanned_date` null, `digest.last_run_date` null.

**Census.** `search_threads in:inbox` (pageSize 50, no next page) returned
26 threads; 26 ≤ 26 threadsTotal; no per-container count exceeds the total.
Already labelled (one of the thirteen on a message's `label_ids` in the
search result), skipped: **20**. Unlabelled, read in full with `get_thread`
PLAIN_TEXT and confirmed to carry none of the thirteen on any message: **6**.

**Proposed dispositions (none applied — see outage below):**

| Thread | Sender / subject | Proposed label | Post-action |
|---|---|---|---|
| 1a07c80a5a923025 | no-reply@accounts.google.com, "Security alert" (Claude for Google Drive access granted) | Security & Verification | label only |
| 1a07c8046c543b3f | noreply-accounts@google.com, "You shared some Google Account data with Claude" | Security & Verification | label only |
| 1a07c681b6c85794 | noreply-lv@email.decathlon.com, "Krājiet punktus, lai saņemtu 3 € kuponu!" | Loyalty (tie-break Loyalty > Promotions; Rimi precedent) | label only; description-widening question still open for Eriks |
| 1a04386997f5d8bc | support@info.printful.com, "❗Price changes are live" | Reply/Do (direct ask: review prices, adjust margins; sibling thread 19ffb512ae9ff08a carries Reply/Do) | label only; Step 2 would task it |
| 1a0436df9d46894f | reply@official.whoop.com, "Introducing Meridian" | Promotions & Ads | ledger row (messageId not in `promotions.csv`, checked) + archive |
| 1a02fe21d1a9f1d8 | do-not-reply@siadokumenti.lv, "Tavi dokumenti SIA reģistrācijai ir gatavi!" (starred) | **none — listed** | unsure between Reply/Do and Family & Personal; 14-day window closed 2026-09-06 |

Needs-Payment: none new (Bite invoice 1a072afdab0b973b already labelled,
skipped; its task is Step 2's). Calendar proposals: none. Receipts rows: none.

**Outage — Gmail writes.** Every `label_thread` call (5, then 1 retry on
1a0436df9d46894f) returned: *"This connector requires additional
permissions. The user needs to reconnect it with the appropriate access."*
Reads (`list_labels`, `search_threads`, `get_thread`) worked throughout, so
the control query proved read scope only. Consequence: 0 labels applied,
0 ledger rows written, 0 threads archived, 0 payment tasks. No Todoist write
either: the daily review task was **not** created because nothing was
labelled and it would have had nothing to review — the proposals above are
the record until the connector is reconnected and `/inbox` rerun; the rerun
is safe (the sweep skips only threads already carrying a label).

**Verification.** `find-tasks` searchText "Review inbox labels" in Personal:
0 tasks (nothing to upsert against). `grep -c 1a0436df9d46894f
ledgers/promotions.csv`: 0 (row never written). No Gmail read-back needed —
no write succeeded.

**Watermarks.** None advanced. `inbox.last_sweep_date` stays null (Step 1 did
not complete its writes — an outage does not advance a watermark);
`mail.last_internaldate_ms` stays 0 (Step 2 has not run; it is triage's
watermark, and Step 1 does not own it); calendar and digest keys untouched.

**Digest hand-off.** Condition met (today ≥ Friday 2026-09-04 16:00 and
`digest.last_run_date` null) but **not run**: `/inbox` is scoped to Steps 0,
1 and close-out, and the digest is its own procedure (`/digest`). Rows with
`digested` empty per the migration log: 17 newsletters, 21 promotions.

**Registry drift.** `config/tools.md` — added the write-scope defect under
Gmail's verified facts. `lessons-learned.md` — one entry appended.

## 2026-09-07 — `/inbox` second attempt (19:12) — Gmail write scope still refused

**Orient.** Gmail `list_labels` populated (`INBOX.threadsTotal` 28,
`messagesTotal` 29). Calendar `list_calendars` populated (8, both swept ids).
Todoist `user-info` = `epetersons87@gmail.com`. Open `[Needs Eriks]`: 0.

**Census.** `search_threads in:inbox` returned 28; 28 ≤ 28. Skipped
(already labelled): 20. Unlabelled: 8 — the six from the earlier entry plus
two new, both read in full with `get_thread` PLAIN_TEXT; the five earlier
label targets re-read (METADATA_ONLY) and confirmed still unlabelled.

New since the earlier read:

| Thread | Sender / subject | Proposed label | Post-action |
|---|---|---|---|
| 1a07ca01b0fcdd03 | hello@info.n8n.io, "Your n8n cloud subscription was cancelled" (body: account and backups removed after three months; export workflows) | Receipts & Subscriptions | receipts row (no amount in body) + archive |
| 1a07c9ff9217215a | help@paddle.com, "Your Subscription for n8n has been canceled." (Cloud Starter, cancelled 7 Sep 2026, access until 23 Sep 2026, ref 74546241-168117680) | Receipts & Subscriptions | receipts row (no amount in body) + archive |

The six earlier dispositions are unchanged (see the previous entry).

**Outage.** All 7 `label_thread` calls and 1 retry returned the same
"requires additional permissions… reconnect" error. `ToolSearch` for
`label_thread` found exactly one Gmail instance
(the same one), so no reconnected connector is available to this session.
0 labels, 0 ledger rows, 0 archives, 0 tasks; review task not created
(nothing to review). Watermarks not advanced. Digest still pending (see
previous entry).

## 2026-09-07 — `/start-day` (19:20) — Steps 0–5; Gmail writes still refused; 5 Todoist tasks, 1 comment, weekly digest produced

**Orient.** Gmail `list_labels` populated (45 labels; `INBOX.threadsTotal` 29,
`messagesTotal` 30). Calendar `list_calendars` populated (8; both swept ids
present). Todoist `user-info` = `epetersons87@gmail.com`; project Personal
24 open tasks at start (21 top-level, 3 subtasks). Ids `_verified`
2026-09-07 — not re-resolved. Open `[Needs Eriks]` at start: 0 open,
0 answered, 0 ambiguous. Watermarks at start: `mail.last_internaldate_ms` 0,
`inbox.last_sweep_date` null, `calendar.last_scanned_date` null,
`digest.last_run_date` null. Note: the `/start-day` skill text points at the
work instance's spec under `Documents/Libernetix AI`; this run followed the
personal `AGENTS.md` loaded from the working directory, per its own
separation rule.

**Step 1 — inbox.** `search_threads in:inbox` (pageSize 50, single page)
returned 29; 29 ≤ 29. Already labelled (one of the thirteen on a message's
`label_ids`), skipped: 20. Unlabelled, read in full with `get_thread`
PLAIN_TEXT: 9 — the 8 from the earlier entries plus one new (LyraBet
1a07cab352b320bb → Promotions & Ads). Dispositions as in the two `/inbox`
entries above plus LyraBet. **Outage:** 8 `label_thread` calls and 1 retry
all returned "This connector requires additional permissions… reconnect".
0 labels, 0 ledger rows (dedupe pre-check: none of the four candidate
messageIds present), 0 archives, review task not created (nothing to
review). `inbox.last_sweep_date` not advanced.

**Step 1.6 — digest.** Condition met (Monday ≥ Friday 2026-09-04 16:00;
`digest.last_run_date` null). Undigested rows read with a CSV parser (the
earlier awk count was wrong because fields contain newlines): newsletters
17 of 1875, promotions 21 of 2544. Thread reads delegated to two read-only
subagents (`get_thread` PLAIN_TEXT only, JSON to scratchpad, fail-loudly
clause); 38 of 38 readable (one transient "service unavailable" on
1a06c9d71352cc80, succeeded on retry; RoboNuggets 1a07572f3cba9b4c body was
links only, bullets drawn from the snippet and say so). Id sets of both JSON
files verified equal to the ledger's undigested rows. Deck built by a local
script with the frontend-slides base CSS: `briefs/digest-2026-09-07.html`,
41 slides (title, 17 cards, divider, 21 cards, end), 55,428 bytes; rendered
in the browser pane (title and card slide checked); sent to chat. Ledgers:
`digested` set to 2026-09-07 on 17 + 21 rows; re-read: undigested 0 and 0,
row totals unchanged (1875, 2544); `git diff --stat` shows exactly 38
changed lines. Data-quality defect noted: migrated `received_date` values
are Excel serials (46269 = 2026-09-04) and `subject` values are raw
MIME-encoded headers — the deck used live thread data; normalise before the
next digest.

**Step 2 — triage.** Mail: 29 inbox threads, all new against watermark 0.
Buckets: Act 2 (Bite invoice; Printful price changes), Know 8 (n8n ×2,
Smartposti, Le-Glue order, MyHeritage, Etsy payout, Revolut tax note, Amex
statement), needs-owner 3 (Amex statements — the rehearsal's open item;
SIA documents; Loyalty scope), Noise 16 (LinkedIn ×5, Skool ×2, Facebook,
Google Maps, Google security ×2, Decathlon, Rimi, Ideabrowser, Emyth, Whoop,
LyraBet). Sent mail, bounded to `after:2026/08/31` (7 days) because the
watermark is 0 — **default applied, logged here**: 3 threads; two are the
earlier automation's self-sent digests (Noise); one is the Malta Certificate
of Conduct thread 1a05c5a7b1dd826f where Eriks sent the last message on
3 Sep → Waiting on others, no task. Calendar: own calendar 1 event (Q&A w/
Nate, 8 Sep 01:00, no prep); Family 8 events, listed in the brief; no
overlaps; no `needsAction` invitations.

Dedupe reads: open tasks 24, completed since 2026-07-09: 5, deleted: 0. No
ref matched.

Writes, each verified with `fetch-object` (ref line, section, labels read
back):
- `6hRVRpcjfmcPGFrx` "Pay Bite Latvija 20.06 EUR" — Backlog, p3, due
  2026-09-20, `size/S`, ref `mail:1a072afdab0b973b`. ✔
- `6hRVRpvCFRMcfwFQ` "Review Printful price changes and adjust store
  prices" — Backlog, p4, `size/M`, ref `mail:1a04386997f5d8bc`. ✔
- `6hRVRq9X62WJ6HMQ` `[Needs Eriks]` Amex statement notices — Waiting /
  Blocked, `agent-waiting`, p4, default: no task. ✔
- `6hRVRqX7GwHq5PPQ` `[Needs Eriks]` SIA registration — Waiting / Blocked,
  `agent-waiting`, p4, default: no task, thread stays unlabelled. ✔
- `6hRVRr7vxmm53x6x` `[Needs Eriks]` Loyalty label scope — Waiting /
  Blocked, `agent-waiting`, p4, default: keep classifying retail loyalty
  as Loyalty. ✔
- Comment `6hRVRrGJfGfw6CcQ` on `6hRHJQwmr656PG6x` "Buy lego glue - Le
  glue" with the Le-Glue order evidence (ref `mail:1a07c78bfa6844cd`);
  `find-comments` beforehand showed only Eriks's own image comment, so not
  a repeat. No move (task already in Waiting / Blocked by Eriks; delivery
  pending) — "possibly done" in the brief. ✔

Census after: 29 open (24 + 5), 3 `agent-waiting` ≤ 29. Deferred, not
created: the vault `wiki_pages:` / `wiki:` drift question from
`procedures/step-3-ingest.md` — raise it when an ingest actually happens.

**Step 3 — vault.** Nothing durable; no snapshot, no vault write.
Considered and declined: SIA-documents notice, Malta conduct-certificate
thread.

**Step 4 — brief.** `briefs/2026-09-07.md` written and delivered in chat.

**Step 5 — watermarks.** `mail.last_internaldate_ms` → 1788797988000
(LyraBet, the newest inbox message processed by triage); `calendar.
last_scanned_date` → 2026-09-07; `digest.last_run_date` → 2026-09-07;
`inbox.last_sweep_date` stays null (Step 1's writes did not complete);
vault snapshot watermark unchanged. Registry drift: none.

## 2026-09-07 — Corrections after the `/start-day` brief (Eriks, in chat)

1. *"open in Gmail in the newsletter doesn't carry the direct thread link, it
   just opens inbox."* Cause: links used `mail.google.com/mail/u/0/#all/<id>`
   (the earlier automation's form); opened from outside Gmail, `u/0` is the
   browser's first Google account and Gmail falls back to its inbox when the
   thread is not there. Fix: link form changed to
   `https://mail.google.com/mail/?authuser=epetersons87@gmail.com#all/<id>`
   in `procedures/step-1-inbox.md` § 3 and `procedures/digest.md`; deck
   rebuilt (38 links in the new form, 0 in the old, verified by grep) and
   re-sent. **Unverified by the assistant** — it has no signed-in Gmail;
   Eriks asked to click one. Existing ledger `threadUrl` values and the two
   task descriptions written today still carry the old form; not rewritten.
2. *"Digest should be just sent once a week or on demand."* The Friday
   hand-off inside Step 1 is withdrawn: `AGENTS.md` § Delivery narrowed in
   place, `procedures/step-1-inbox.md` § 6 marked WITHDRAWN,
   `procedures/digest.md` narrowed. The digest now runs only on `/digest`.
   A fixed weekly trigger would be a scheduled `/digest`, offered to Eriks,
   not set up. Two entries appended to `lessons-learned.md`.

## 2026-09-07 — Step 1 completed after Gmail reconnect (19:43–19:46)

Eriks reconnected the Gmail connector (the app's tool-permission panel had
shown every write tool as "Always allow" throughout; the missing piece was
the Google OAuth grant — see `config/tools.md` RESOLVED note and
`lessons-learned.md`). Eriks: *"try now"*. Same connector instance id.

**Control write.** `label_thread` on 1a0436df9d46894f (Whoop) with
Promotions & Ads → `{}`; `get_thread` METADATA_ONLY read back
`Label_6413919896574930163` on the message. Inbox re-listed: 29 threads,
29 ≤ `INBOX.threadsTotal` 29; one thread (Google "Security alert"
1a07c80a5a923025) had gained a second message at 16:42:46Z — "You allowed
Claude for Gmail access", the reconnect itself — re-read in full, same
class.

**Labels applied, each read back on every message (8 threads):**
LyraBet 1a07cab352b320bb → Promotions & Ads ✔; n8n 1a07ca01b0fcdd03 →
Receipts ✔; Paddle 1a07c9ff9217215a → Receipts ✔; Google
1a07c8046c543b3f → Security ✔; Google 1a07c80a5a923025 (2 messages) →
Security ✔; Decathlon 1a07c681b6c85794 → Loyalty ✔; Printful
1a04386997f5d8bc → Reply/Do ✔; Whoop 1a0436df9d46894f → Promotions ✔.
Left unlabelled: SIA documents 1a02fe21d1a9f1d8 (open question).

**Ledger rows (dedupe on messageId, each read back):** promotions +2
(LyraBet 2026-09-07, `list_unsubscribe: body-cue`; Whoop 2026-08-27),
receipts +2 (n8n and Paddle, no amount — not in body; Paddle ref
74546241-168117680 as invoice_no). `git diff --stat`: 2 + 2 insertions.

**Archived (INBOX removed, read back absent):** Whoop, LyraBet, n8n,
Paddle — 4. `list_labels` after: `INBOX.threadsTotal` 25 = 29 − 4;
Promotions & Ads threadsTotal 2728 → 2730, Receipts 668 → 670, Security
215 → 217, Loyalty 30 → 31, Reply/Do 95 → 96 — every delta matches.

**Review task.** `6hRVc6H632PVx4cQ` "[Act] Review inbox labels —
2026-09-07", Backlog, p3, due today, counts in the description;
`fetch-object` read back. Project Personal now 30 open tasks.

**Watermark.** `sources.gmail.inbox.last_sweep_date` → 2026-09-07 (Step 1
completed against a populated control). Other keys unchanged.

## 2026-09-07 — Correction: payment tasks land in This Week (19:55)

Eriks, in chat: *"For any e-mail that are required payments - always put in
THIS WEEK column in todoist."* Applied as a CORRECTED marker in
`config/routing-rules.md` § The payment task (authoritative copy), with
pointer annotations in `AGENTS.md` § Security boundary ("create tasks (in
Backlog)") and `procedures/step-2-triage.md` § 4 ("where every new task
lands"). Each read back by grep. Existing task `6hRVRpcjfmcPGFrx` "Pay Bite
Latvija 20.06 EUR" (top-level, no parent) moved Backlog → This Week with
`update-tasks` (sectionId only); `fetch-object` read back `sectionId`
6hJQ557XXQ7fRjVQ. Lessons entry appended.

## 2026-09-07 — Carve-out 5 recorded: completed payment task → Paid label + archive (20:05)

Eriks asked in chat whether closing a payment task relabels and archives its
mail; it did not. The cost (a mistaken completion is acted on the same way;
the mail is handled at the next run, not at the click) was named; Eriks:
*"it's okay, I won't click by mistake. worst case, they (whoever that is)
will send a reminder later on."* "Change the label" read as swap
(add `Paid`, remove `Needs-Payment`), his words. Edits, each read back by
grep: `AGENTS.md` § Phase gates carve-out 5, § Security boundary
allowed/forbidden annotations; `config/routing-rules.md` § The payment task;
`config/sources/gmail.md` allowed-write 4 and forbidden annotation;
`config/tools.md` Gmail write line; `procedures/step-2-triage.md` § 3b (the
steps); `state/state.json` `sources.gmail.paid_label_id` =
Label_2307425248756940905 (from today's `list_labels`, name
`Finance & Accounts/Paid`, 209 threads). No thread acted on now: the only
completed `Pay …` tasks in the last 60 days (two Margosik WhatsApp payments)
carry no `ref: mail:` line, so they are out of class.

## 2026-09-08 — `/start-day` run (09:42–09:50 Europe/Riga)

**Step 0 — orient.** Governing files and `lessons-learned.md` read (8
entries, all applied; `config/methods.md` empty). Gmail live: `list_labels`
46 labels, `INBOX.threadsTotal` 10. Calendar live: `list_calendars` 8
calendars, both swept ids present. Todoist live: `user-info` =
epetersons87@gmail.com. Ids `_verified` 2026-09-07, not re-resolved.
`agent-waiting` tasks in Personal: 3 (`6hRVRq9X62WJ6HMQ`,
`6hRVRqX7GwHq5PPQ`, `6hRVRr7vxmm53x6x`); `find-comments` on each: 0
comments. Open 3, answered 0, ambiguous 0; defaults stay. Not a rerun:
`calendar.last_scanned_date` was 2026-09-07 and the inbox held 6 threads
newer than `mail.last_internaldate_ms` 1788797988000.

**Step 1 — inbox.** `search_threads in:inbox` → 10 threads, 10 ≤ 10.
Skipped, already labelled (label ids read off messages): 3 —
1a072afdab0b973b Bite (Needs-Payment), 19ffb512ae9ff08a Printful
(Reply/Do), 19ecbccd34e3286c Revolut (Banking & Cards). Left unlabelled:
1a02fe21d1a9f1d8 SIA documents (open question). Read in full
(PLAIN_TEXT) and labelled, each read back on every message with
METADATA_ONLY:
- 1a07fb1582a79817 NEXT.io → Newsletters & Learning ✔
- 1a07f33afd738675 Skool weekly digest → Newsletters & Learning ✔
- 1a07f2f0c19f46ab EMyth → Promotions & Ads (unsure newsletter vs
  promotion; tie-break → Promotions) ✔
- 1a07fa5ced288ed1 LinkedIn invite reminder → Professional Networking ✔
- 1a07d7b5f870f2af IHG One Rewards → Loyalty (hotel loyalty programme;
  Loyalty > Promotions) ✔
- 1a07dd9a6eefff21 Google Calendar notification "Q&A w/ Nate" → Schedule
  Calendar ✔. Sub-rule did not fire (not a daily agenda / webinar /
  bilesuserviss). Match check: `list_events` on both calendar ids,
  2026-09-07T21:55Z–22:35Z → own calendar returned event
  `ib19cpad1t5oeu0u137e1si258` "Q&A w/ Nate" 01:00–01:30 Riga, organiser
  Eriks; Family calendar empty → handled.
Ledger rows (dedupe on messageId — grep 0 hits before; each read back by
tail; `git diff --stat` 3 insertions): newsletters +2 (NEXT.io, Skool,
both `body-cue`), promotions +1 (EMyth, `body-cue`). Link form
`?authuser=…#all/<id>`. Archived (`unlabel_thread ["INBOX"]`, read back
INBOX absent): NEXT.io, Skool, EMyth, Q&A notification — 4. Review task
`6hRg233R2fccHJqQ` "[Act] Review inbox labels — 2026-09-08", Backlog, p3,
due today; read back via `find-tasks` on the Backlog section (16 tasks,
present). Payment tasks 0, calendar proposals 0.

**Step 2 — triage.** Inbox: 6 new threads (all > watermark), all Noise /
Know; 0 tasks. Sent (`in:sent after:2026/09/06`): 2 threads —
1a067fe7acd48584 "Re: Pulciņa rēķins" (Eriks replied "Paid" 2026-09-07
17:54:42Z to margaritaeliya@gmail.com; Lidl Arena / My Academy corrected
invoices for Ervins and Marks; two PDFs, unreadable via connector; the
message says payment proof can be filed with the annual income-tax
declaration — Know, surfaced as a candidate, no task under the standing
bias) and 1a079063da173179 (forward of a newsletter to
daisyqdesign@gmail.com; Noise). Calendar, both ids, 2026-09-08 00:00 →
2026-09-15 23:59 Riga: own calendar 1 event (Q&A w/ Nate, 01:00); Family
calendar 10 events (listed in the brief); no overlaps; no `needsAction`
invitations; no prep tasks. Dedupe reads: `find-tasks` project Personal
26 open (totalCount 26, hasMore false; 3 agent-waiting ≤ 26);
`find-completed-tasks` since 2026-07-10: 9; `find-activity` deleted since
2026-07-10: 0. No movement evidence on any open task; no moves; no
comments.

**Step 2 § 3b — closed payment tasks (carve-out 5).** In class: 1 —
`6hRVRpcjfmcPGFrx` "Pay Bite Latvija 20.06 EUR", `checked: true`,
completed 2026-09-07T17:49:27Z, first description line
`ref: mail:1a072afdab0b973b`. Out of class: the two Margosik WhatsApp
`Pay …` tasks (no `ref: mail:` line). `get_thread` METADATA_ONLY on
1a072afdab0b973b: 1 message, labels UNREAD, Needs-Payment, INBOX → in
class. Step 1: `label_thread` Paid (Label_2307425248756940905) → `{}`;
read back: labels Paid, UNREAD, Needs-Payment, INBOX ✔. Step 2:
`unlabel_thread` Needs-Payment → **refused by the Claude Code auto-mode
classifier** ("Blocked by classifier"), not a Gmail error. Per § 3b
stopped at the first failed step; no retry; INBOX not touched. Thread
state: Paid + Needs-Payment + INBOX. The same response also refused a
routine `find-tasks` read (searchText) — a second read with different
parameters succeeded, so this is the desktop app's permission layer, not
a connector outage. Idempotent on rerun: the thread still carries
Needs-Payment, so § 3b re-fires; `label_thread` Paid on an already-Paid
thread is a no-op.

**Step 3 — vault.** Nothing durable; no snapshot, no vault write.
Considered and declined: Lidl Arena invoices (receipts), the Q&A
notification (calendar).

**Step 4 — brief.** `briefs/2026-09-08.md` written and delivered in chat.

**Step 5 — watermarks.** `mail.last_internaldate_ms` 1788797988000 →
1788848723000 (NEXT.io, newest message processed);
`calendar.last_scanned_date` → 2026-09-08; `inbox.last_sweep_date` →
2026-09-08; vault snapshot watermark and `digest.last_run_date`
unchanged. Registry drift: none — every tool used (`list_labels`,
`search_threads`, `get_thread`, `label_thread`, `unlabel_thread`,
`list_calendars`, `list_events`, `user-info`, `find-tasks`,
`find-comments`, `find-completed-tasks`, `find-activity`, `add-tasks`) is
listed in `config/tools.md`. Defect noted for the tools file: the
`unlabel_thread` refusal came from the app's permission classifier, a
layer the registry's "If broken" lines do not name; add when it recurs.

## 2026-09-08 — Corrections after the `/start-day` brief (Eriks, in chat, 09:55)

1. *"When you add review labels tasks, don't just mark it for today, but also
   move it THIS WEEK column, same as with the payments."* Applied: CORRECTED
   marker in `procedures/step-1-inbox.md` § 5 (authoritative for the step),
   NARROWED annotation in `config/routing-rules.md` § The payment task (the
   "one task class" sentence now names two), pointer annotations in
   `AGENTS.md` § Security boundary and `procedures/step-2-triage.md` § 4.
   Today's task `6hRg233R2fccHJqQ` (top-level, no parent) moved Backlog →
   This Week with `update-tasks` (sectionId only); `find-tasks` on the This
   Week section read back 5 tasks including it, `sectionId`
   6hJQ557XXQ7fRjVQ.
2. *"For calendar events / invites. I want only real people ones. For example
   one with Nate is the advertising. You can ask in such cases to be sure."*
   Applied as a new bullet in `config/routing-rules.md` § Source hints ›
   Calendar (the authoritative copy: real-person test, advertising handling,
   ask-when-unsure with default *not listed*), a WIDENED note on the Schedule
   Calendar sub-rule in § Mail label taxonomy, and pointers in
   `procedures/step-2-triage.md` § 2, `procedures/step-4-brief.md` § 6 and
   `config/sources/calendar.md`. Not changed: the archived notification thread
   1a07dd9a6eefff21 keeps its Schedule Calendar label (labels are never
   removed; it is out of the inbox either way), and the calendar event itself
   is untouched (calendar is read-only). Two entries appended to
   `lessons-learned.md`. Each edit verified by exact-string read-back.

## 2026-09-08 — Carve-out 5 completed on the Bite thread (Eriks, in chat, 10:05)

Eriks: *"Ok, let's fix now this label changing / removing issue you had with
Bite."* Fresh read of 1a072afdab0b973b: Paid, UNREAD, Needs-Payment, INBOX
(unchanged since the run). `unlabel_thread` Needs-Payment → `{}`; read back:
Paid, UNREAD, INBOX ✔. `unlabel_thread` INBOX → `{}`; read back: Paid,
UNREAD ✔. `list_labels` after: Needs-Payment threadsTotal 2 → 1, Paid 210
(unchanged — the label was already on it). The morning's refusal did not
recur; the same call succeeded on Eriks's explicit ask. Task
`6hRVRpcjfmcPGFrx` untouched (already completed by Eriks). Note for the next
run: `INBOX.threadsTotal` reads 8 (was 6 after the run, 5 expected after
this archive) — three messages arrived since the sweep (UNREAD 10115 →
10118); they are new mail for the next run, not a discrepancy in this one.

## 2026-09-08 — `/inbox` run (15:27–15:35 Europe/Riga)

**Step 0.** Gmail live: `list_labels` 46 labels, `INBOX.threadsTotal` 14.
Calendar live: 8 calendars, both swept ids present. Todoist live:
`user-info` = epetersons87@gmail.com. `agent-waiting` tasks in Personal: 3,
`find-comments` on each: 0 — open 3, answered 0, ambiguous 0; defaults
stay.

**Step 1.** `search_threads in:inbox` → 14 threads, 14 ≤ 14. Skipped,
already labelled (ids read off messages): 4 — 1a07fa5ced288ed1 LinkedIn
Hazeb (Professional Networking), 1a07d7b5f870f2af IHG (Loyalty),
19ffb512ae9ff08a Printful (Reply/Do), 19ecbccd34e3286c Revolut (Banking &
Cards). Left unlabelled on purpose: 1a02fe21d1a9f1d8 SIA (open question)
and 1a07ff008d25cd60 Supabase "project ChallengeFinds is going to be
paused" (unsure Reply/Do vs Noise — platform notice with a real
account-owner decision inside a 90-day window; listed, not guessed). Read
in full (PLAIN_TEXT) and labelled, each read back on every message:
- 1a080f5ae9698874 Elektrum invoice → Needs-Payment ✔. Body: 100.15 EUR,
  "Rēķina apmaksas termiņš: 30.09.2026.", invoice 877797690561, contract
  87730445301, Dzintaru iela 8. Dedupe: `find-tasks` project Personal 27
  open (hasMore false), `find-completed-tasks` since 2026-07-10 9,
  `find-activity` deleted 0 — no `ref: mail:1a080f5ae9698874`. Task
  `6hRhm7h4x22gqRhx` "Pay Elektrum 100.15 EUR", This Week, p3 (due beyond
  7 days), size/S, due 2026-09-30; read back via `find-tasks` on the This
  Week section (6 tasks, present, description first line is the ref).
- 1a080f1e7c12e29f Productivity Game → Newsletters & Learning ✔
- 1a08039b3f8efa34 Online Gambling Quarterly → Newsletters & Learning ✔
- 1a08013b06076a6e LinkedIn PYMK → Professional Networking ✔
- 1a08012062e6caab Audible trial → Promotions & Ads ✔
- 1a0800c961da3516 CuriosityStream → Promotions & Ads ✔
- 1a07fe54cc14a465 LitRes → Promotions & Ads ✔
- 1a07fcd7140fc49a Canva Enterprise cold sales pitch → Promotions & Ads ✔
  (a named sender, but a sales campaign with an unsubscribe cue).
Ledger rows (grep 0 dupes before; each read back by tail; `git diff
--stat` 6 insertions): newsletters +2, promotions +4, all `body-cue`.
Archived (`unlabel_thread ["INBOX"]`): Productivity Game, OGQ, Audible,
CuriosityStream, LitRes, Canva — 6; read-backs recorded below. Review
task `6hRg233R2fccHJqQ` found by title, description replaced to cover
both of today's sweeps (`update-tasks` returned the new text). Calendar
proposals: 0. Payment tasks: 1.

**Close-out.** `inbox.last_sweep_date` already 2026-09-08 (unchanged).
`mail.last_internaldate_ms` not advanced — that is Step 2's key and
triage did not run; the Elektrum message (1788869976000) is newer than
the stored 1788848723000 and will be triage's first new item next run
(its ref already has a task, so dedupe absorbs it). Registry drift: none.

## 2026-09-08 — Ad-hoc: vitamin D daily task and brief reminder (Eriks, in chat, ~23:58)

Eriks: *"I completely forgot. I need to start drinking 8000DV (vitamin D)
daily for 3 months - please add it to my todoist daily and remind me about
it in the briefs."* Dedupe: `find-tasks` project Personal, searchText
"vitamin" → 0. Created task `6hRpMx6gfFHWRcfQ` "Take vitamin D (8000DV)",
This Week (`6hJQ557XXQ7fRjVQ`), p4, `size/XS`, description first line
`ref: chat:2026-09-08-vitamin-d`; `fetch-object` read-back: `dueDate`
2026-09-08, `recurring` "every day ending 2026-12-08", `sectionId` matches.
Section choice: This Week, by analogy with Eriks's 2026-09-08 instruction
that dated review tasks sit in This Week, not Backlog — Eriks did not name
a section; flagged in chat. Dose written as Eriks wrote it ("8000DV");
"IU" not assumed. Files: `config/routing-rules.md` new § Standing
reminders (one active row, end 2026-12-08); `procedures/step-4-brief.md`
new § 6a; `lessons-learned.md` one entry. No Gmail, calendar or vault
writes.

## 2026-09-09 — `/start-day` run (11:06–11:20 Europe/Riga)

**Step 0.** Gmail live: `list_labels` 46 labels, `INBOX.threadsTotal` 11.
Calendar live: `list_calendars` 8 calendars, both swept ids present.
Todoist live: `user-info` = epetersons87@gmail.com, user 22613842. Ids
`_verified` 2026-09-07 (2 days) — not re-resolved. `agent-waiting` tasks in
Personal: 3, `find-comments` on each: 1 comment each, none with the
assistant marker → open 3, answered 3, ambiguous 0.
- `6hRVRq9X62WJ6HMQ` Amex — Eriks: *"No task, this is just notification. I
  don't use it for long time, but it just keeps coming since the card itself
  is still active."* → rule added `config/routing-rules.md` § Source hints ›
  Mail (line 85, read back). Closing comment posted; completed.
- `6hRVRqX7GwHq5PPQ` SIA — Eriks: *"Company is registered. All done."* →
  option (a), no task. Thread 1a02fe21d1a9f1d8 read (`get_thread`): no
  `label_ids` at all — not in INBOX, not STARRED any more. Label not applied:
  the allowed write is on an inbox thread; failed closed. Closing comment
  posted; completed.
- `6hRVRr7vxmm53x6x` Loyalty — Eriks: *"a)"* → Loyalty row widened in
  `config/routing-rules.md` § Mail label taxonomy (line 206, superseded text
  quoted, read back). Closing comment posted; completed.
`complete-tasks` returned all three; `find-tasks agent-waiting` afterwards
returned only the new Supabase question (then 2 after the vault question).
Idempotency: `calendar.last_scanned_date` 2026-09-08 ≠ today; mail newer
than 1788848723000 present → full run.

**Step 1.** `search_threads in:inbox` → 11 threads, 11 ≤ 11. Skipped, already
labelled (ids read off messages): 3 — 1a080f5ae9698874 Elektrum
(Needs-Payment), 19ffb512ae9ff08a Printful (Reply/Do), 19ecbccd34e3286c
Revolut (Banking & Cards). Labelled, each `label_thread` → `{}` and read
back on every message:
- 1a085304965f8d0b Facebook friend update → Social Media ✔ (sender domain
  decisive; body not read).
- 1a084f8c7ad692c9 NEXT.io newsletter → Newsletters & Learning ✔; row
  appended to `ledgers/newsletters.csv` line 6212 (0 dupes before, read
  back); `unlabel_thread INBOX` → `{}`; read back: label only ✔.
- 1a084d1f3a7e8531 LitRes promo → Promotions & Ads; `get_thread` read-back
  failed 2× "The caller does not have permission"; `search_threads
  in:anywhere from:info@nsl.litres.ru after:2026/09/08 includeTrash` returned
  it with `label_ids` UNREAD, Promotions & Ads, **TRASH** (historyId
  14176083, i.e. trashed during the run, after the label). Row appended to
  `ledgers/promotions.csv` line 10073 (0 dupes before, read back). No archive
  step (already out of the inbox by someone else's act).
- 1a08533da23e4a09 "New event: Yoga home" and 1a0853412c516cf2 "Cancelled
  event: Yoga home" (both margaritaeliya@gmail.com, Google Calendar
  notifications, `.ics` attached, Family calendar) → Schedule Calendar ✔ ✔.
  Real-person test: Family-calendar entry → real, not advertising. Match
  check: `list_events` both calendars 16 Sep 09:25–10:35 `fullText: Yoga
  home` → empty on both (the event was cancelled 15 s after creation). No
  proposal (source withdrew it); no archive (carve-out 4 needs a match).
Left unlabelled (3): 1a0813348da28051 Le-Glue (Eriks sent last, 17:22Z 8
Sep; live conversation kept re-sweepable); 1a07ff008d25cd60 Supabase (now
`[Needs Eriks]` `6hRvw46QX46528XQ`, ref line read back via `fetch-object`);
1a0844c180ffb6a3 Glassdoor (`get_thread` PLAIN_TEXT failed 3× "does not
have permission"; `search_threads in:anywhere … includeTrash` shows UNREAD,
**TRASH**, historyId 14175968 — trashed during the run, before any label).
Payment tasks: 0. Calendar proposals: 0. Review task `6hRvwQr3m62FCXHQ`
"[Act] Review inbox labels — 2026-09-09", This Week, p3, `dueDate`
2026-09-09 (returned object checked; no prior task by that title in the 27
open). `list_labels` at close: INBOX threadsTotal 10 (11 − NEXT.io −
Elektrum − 2 trashed + 3 arrived: 1a0853aa0510037f LinkedIn "Khushi",
1a085390f0a4865a Supabase "has been paused", 1a08535dba44ab62 Audible —
all after the sweep read, left for next run); TRASH 218 → 220; Paid 210 →
211; Needs-Payment 2 → 1; Schedule Calendar 272 → 274; Social Media 11 →
12; Newsletters 1952 → 1953; Promotions 2735 (unchanged — a trashed thread
keeps its label but the count reads the same).

**Step 2.** Mail: 11 inbox threads read; new since 1788848723000: 8
(Elektrum, Supabase, Le-Glue, Glassdoor, LitRes, NEXT.io, Facebook, Yoga
×2 — Le-Glue newest 1788888128000 is Eriks's own reply). Sent since
2026/09/08: 1 thread (Le-Glue), Eriks's own messages carry no new
commitment beyond "please refund … I'll try to find some suppliers in
Europe" (conditional; no task). Buckets: Know 6, Noise 1, needs-owner 1
(Supabase), movement 1 (Le-Glue). Dedupe reads: `find-tasks` project 27
open (hasMore false), `find-completed-tasks` since 2026-09-03 7,
`find-activity deleted` since 2026-07-11 0. Tasks created from sources: 0.
`[Needs Eriks]` created: 2 — `6hRvw46QX46528XQ` Supabase (p4, Waiting /
Blocked, default no task) and `6hRvwHmgrvg8jRHQ` vault `wiki:` vs
`wiki_pages:` (p4, default `wiki:` for new files; read back). Comment on
`6hRHJQwmr656PG6x` "Buy lego glue" (`6hRvw4JxmhJPQx5x`, marker + ref, prior
comments checked: different evidence) — no move, already Waiting / Blocked,
no parent. Second comment on the Supabase question (`mail:1a085390f0a4865a`
arrived mid-run: "has been paused").
Calendar: own calendar 9–16 Sep empty; wider control 10 Aug – 9 Oct → 14
events (populated; "Q&A w/ Nate" ×3 advertising, all past). Family 9–16 Sep
→ 10 events, listed in the brief; overlaps 0; needsAction 0; advertising 0;
tasks 0 ("Взять камни" on Массаж not addressed to Eriks). Recurring series
named in `config/routing-rules.md` § Calendar scope (line 162, assistant
edit per § Source hints, read back).
**§ 3b carve-out 5:** completed tasks with `Pay ` + `ref: mail:`: Bite
(thread already handled 2026-09-08, not re-read) and Elektrum
`6hRhm7h4x22gqRhx` (completed 2026-09-08T20:12:35Z). Elektrum thread
1a080f5ae9698874 `get_thread METADATA_ONLY`: UNREAD, Needs-Payment, INBOX →
in class. `label_thread Paid` → `{}`; read back: Paid, UNREAD,
Needs-Payment, INBOX ✔. `unlabel_thread Needs-Payment` → `{}`; read back:
Paid, UNREAD, INBOX ✔. `unlabel_thread INBOX` → `{}`; read back: Paid,
UNREAD ✔. No permission refusal this time.

**Step 3.** Vault `AGENTS.md` read. Nothing durable: SIA thread considered
(one-line fact, no company name, no personal-admin pages in the vault) and
declined. The `wiki_pages:` drift flagged in the procedure since 2026-09-07
raised as a `[Needs Eriks]` question (above). No vault writes.

**Step 4.** Brief written to `briefs/2026-09-09.md` and delivered in chat.

**Anomaly.** Two threads (LitRes, Glassdoor) moved to TRASH mid-run by an
actor other than this run (history ids inside the run window; the run made
no trash-capable call). Reported in the brief; nothing done to them.
Registry: `config/tools.md` gains the `get_thread`-on-TRASH fact (line
140). `search_threads from:… newer_than:2d` also failed to return the
trashed LitRes thread — the `in:anywhere`/`includeTrash` form did.

**Close-out.** Watermarks: `mail.last_internaldate_ms` 1788848723000 →
1788941177000 (Yoga cancelled, newest processed; the 3 mid-run arrivals are
above it); `calendar.last_scanned_date` → 2026-09-09;
`inbox.last_sweep_date` → 2026-09-09; vault and digest keys unchanged.
Lessons: none appended (no correction from Eriks this run).

## 2026-09-09 — Corrections from Eriks in chat (11:25–11:32 Europe/Riga)

Eriks, three sentences: (1) *"Promotions should also be logged and archived."*
— already the rule (taxonomy row: row + archive); today's LitRes thread was
in Trash by another actor before the archive step. No change; explained in
chat. (2) *"Same goes with calendar invites - check if those exist, if not,
create one."* → **carve-out 7**: an unmatched, uncancelled real invite or
booking is created with `create_event` on Eriks's own calendar, no
attendees, `ref: mail:<thread_id>`, read back, logged, then archived;
unsure → `[Needs Eriks]`, default not created. (3) *"The notifications from
google should be ignored (those are usually about the event creation or
deletion) so those can be deleted immediatlly."* → **carve-out 6**: Google
Calendar notification threads (New/Updated/Cancelled event, Invitation,
Accepted/Declined, daily agenda) are trashed with `trash_thread` after the
existence check; nothing else is ever trashed. Assistant's reading, stated
in chat for Eriks to correct: daily-agenda mails are included in the class
(they are Google notifications), which withdraws the daily-agenda →
Promotions half of the Schedule sub-rule.

Files edited, each by exact-match replace and read back by grep:
`AGENTS.md` (§ Security boundary allowed/forbidden ×3, § Phase gates
disabled-list, carve-out 4, carve-outs 6 and 7 added at lines 315/334),
`config/routing-rules.md` (Schedule Calendar row), `config/sources/gmail.md`
(tools, allowed-write 5, forbidden, intro), `config/sources/calendar.md`
(allowed writes ×2), `config/tools.md` (Gmail write/never-used, Calendar
never-used), `procedures/step-1-inbox.md` (§ 3 Schedule Calendar, § 4),
`procedures/step-4-brief.md` (§ 4), `lessons-learned.md` (+2 entries).
Superseded text quoted in place throughout.

Action on Eriks's words: `trash_thread` 1a08533da23e4a09 → `{}`,
1a0853412c516cf2 → `{}`; read back via `search_threads in:anywhere
from:margaritaeliya@gmail.com after:2026/09/08 includeTrash`: both carry
Schedule Calendar + TRASH, no INBOX (history ids 14176544/14176547).
Existence check had already run (empty on both calendars; event cancelled
by its source). Review task `6hRvwQr3m62FCXHQ` description updated with the
addendum (`update-tasks` returned the new text). Brief file addendum
appended. No calendar write (nothing to create).

Note for maintenance: `AGENTS.md` is 5,553 words against its stated
3,000-word target (it was over before today); the carve-out text is the
growth. Not trimmed now — a candidate to move § Phase gates carve-outs into
a procedure file, on Eriks's yes.

## 2026-09-09 — ad-hoc: TRUS booking (chat)

Eriks booked transrectal prostate USG themself on piearsta.lv: 18.09.2026
10:30, Republikas laukuma klīnika, Republikas laukums 3-18, Dr V. Šalajevs,
service "Ultrasonoskopija prostatai", €70. Confirmation mail in inbox:
thread `1a085a1af2a3db83` (pieraksti@piearsta.lv, "Tavs pieraksts ir
apstiprināts!", internalDate 1788948360000); application receipt thread
`1a08582e68045a8d`. Research comment `6hRp9m9VXrGmrmhx` on task
`6hRV8xr4fHJP3gQQ` was written 2026-09-08 and read back.

Outages this session: Todoist `add-comments` and `update-tasks` rejected
every call with "expected array, received string" (3 attempts each,
schemas loaded as opaque `{type: object}`), so the booking comment and the
move to Done were NOT written. Calendar `list_events` and `search_events`
failed on every call (5 attempts, both calendars); `list_calendars`
succeeded, so the connector is alive but event reads are down. Conflict
check for 18.09 10:30 therefore not done. No calendar write attempted
(needs Eriks's per-item yes). Task completion awaits Eriks's yes naming it.

Addendum, same day, after Eriks's yes in chat — *"yes, update the calendar
and todoist - you can spawn new agents / chats for this"* — answering the two
numbered questions (complete task 6hRV8xr4fHJP3gQQ; create the calendar
event). Subagent a646a1a49e415f432 executed with a fully specified brief:
`create_event` on `epetersons87@gmail.com`, no attendees → event
`vmenfco7t7tf96qr0fb2cl7vt0`, "Transrektālā USG — Republikas laukuma
klīnika", 2026-09-18T10:30:00+03:00 → 11:00, description ends
`ref: mail:1a085a1af2a3db83`. Manager read it back via `search_events`
("Transrektālā USG"): one event, id/summary/start/end match, no attendees,
status confirmed. Working calendar param shape is the one in
`config/sources/calendar.md:54-56` (startTime/endTime), not timeMin/timeMax
— the earlier "outage" was partly a wrong param name on the manager's side.
Conflict found on the Family calendar the same morning: "A KLINIKA "
10:00–13:00 (creator daisyqdesign@gmail.com); surfaced to Eriks as data.
Todoist comment and completion still NOT written: `add-comments` and
`complete-tasks` reject array input in this session (schemas load empty);
no API token exists outside the MCP connector. Handed to a fresh session via
a spawn-task chip carrying Eriks's words.

## 2026-09-09 — ad-hoc: TRUS task closed (chat, fresh session)

Eriks, in chat, naming the item: *"Complete Todoist task 6hRV8xr4fHJP3gQQ
"Book Transrectal ultrasound — Transrektāla USG" in project Personal. I
said yes to this on 2026-09-09."* Carve-out 2 (per-item yes naming the
task). Follows the hand-off in the entry above.

Pre-checks: `fetch-object` → projectId `6hJQ53x8Pjpr9rJQ` (= Personal in
state), sectionId `6hJQ557XXQ7fRjVQ` (This Week), `checked: false`;
`find-comments` → 1 existing comment (research `6hRp9m9VXrGmrmhx`).

Writes, in order, each read back:
1. `add-comments` → comment `6hRwXjm592Rx4CVx` (postedAt
   2026-09-09T10:21:46Z): booked 18.09.2026 10:30, Republikas laukuma
   klīnika, Republikas laukums 3-18, Dr Vladimirs Šalajevs,
   "Ultrasonoskopija prostatai", €70, tel 28002363, `ref:
   mail:1a085a1af2a3db83`, calendar event `vmenfco7t7tf96qr0fb2cl7vt0`,
   Family-calendar overlap "A KLINIKA" 10:00–13:00 noted. Read back via
   `find-comments commentId` — content matches.
2. `complete-tasks ["6hRV8xr4fHJP3gQQ"]` → successCount 1, failureCount 0.
   Read back via `fetch-object` → `checked: true`, completedAt
   2026-09-09T10:21:53.031Z.

No other task touched. No Gmail or Calendar write (the event already
existed; thread 1a085a1af2a3db83 left as is — not a Needs-Payment thread,
so carve-out 5 does not apply).

Connector note: the `add-comments` / `complete-tasks` "expected array,
received string" failures in the previous session did not recur. Loading
the four tools through `ToolSearch select:<name>,…` returned full schemas
and both accepted array input first time. Lesson appended.

## 2026-09-09 — ad-hoc: Le-Glue refund, Block Lock reorder (chat)

Eriks, in chat: *"Le Glue refunded transaction. Reordered via Block Lock.
Check the e-mail"* (Gmail web link to the refund thread).

Read: `search_threads` Le-Glue newer_than:10d → 4 threads; Block Lock →
1 thread. `get_thread` PLAIN_TEXT on both new threads:
- `1a0862502f1d4882` "Refund notification", Shopify for Le-Glue, 12:29Z,
  INBOX, unlabelled — order #7334 refunded in full, €105.15, "up to 10 days".
- `1a0863ff623d94e1` "Order #1359 confirmed", Block Lock Toy Glue, 12:58Z,
  INBOX, unlabelled — €41.90 (€23.95 + €17.95 shipping), standard shipping
  up to 10 working days.
Todoist `find-tasks` first returned HTTP 401 (retry_after 1280); the
`fetch-object` retry ~1 min later succeeded — transient, not an outage.
Task `6hRHJQwmr656PG6x` "Buy lego glue - Le glue": Waiting / Blocked
(`6hJQ57R2hvM9RQgQ`), `checked: false`.

Defect found: order thread `1a07c78bfa6844cd` (#7334, 7 Sep) carries the
Receipts label and is archived, but `ledgers/receipts.csv` had no row for
it (grep on messageId, 7334, "glue", 105.15 → 0 hits). Backfilled below.

Writes, each read back:
1. `label_thread` Receipts & Subscriptions on `1a0862502f1d4882` and
   `1a0863ff623d94e1` → `{}` each.
2. `ledgers/receipts.csv` +3 rows (528 → 531 lines): #7334 order backfill
   (105.15, dated 2026-09-07, note says backfilled), #7334 refund (-105.15),
   Block Lock #1359 (41.90). Read back by grep on the three messageIds.
3. `unlabel_thread ["INBOX"]` on both threads (carve-out 4, Receipts after
   ledger row). Read back via `search_threads` METADATA_ONLY: both show
   `labelIds: ["Label_8316160478815561067"]` only — INBOX absent.
4. `add-comments` on `6hRHJQwmr656PG6x` → `6hRxPP8xfXqV24vQ` (postedAt
   2026-09-09T13:01:55Z): refund + reorder evidence, both `ref: mail:` ids,
   watch-items (credit by 19 Sep, Block Lock dispatch). Read back via
   `find-comments commentId` — content matches. No section move (already
   Waiting / Blocked; still waiting, now on Block Lock), no completion.

Not touched: conversation thread `1a0813348da28051` "Message from Le-Glue"
(still in INBOX, unlabelled, Eriks's own reply is the newest message) — now
moot after the refund; left for the next sweep. No task created for the
refund check; offered in chat. Watermarks unchanged (ad-hoc, not a run).

## 2026-09-09 — ad-hoc: refund watch task (chat)

Eriks, in chat, on the offer above: *"Yes, add the 19 SEP watch task. It
shouldn't be this week though. Place it in This Month column."*

Write: `add-tasks` → `6hRxmG7HFGvq6Pwx` "Check Le-Glue refund of €105.15
has landed (order #7334)", project `6hJQ53x8Pjpr9rJQ`, section This Month
`6hJQ55cfhvj9Hq6Q` (Eriks's explicit placement, overriding the Backlog
default), due 2026-09-19, p4, description with `ref: mail:1a0862502f1d4882`,
the related task id and Eriks's words. Read back via `fetch-object`
(section, due date, description checked). No other write.

## 2026-09-09 — ad-hoc: podiatrist shortlist (chat)

Eriks, in chat: *"let's work on the Book podiatrist appointment - find me
podiatrists near me"*. Todoist read first: `find-tasks` searchText
"podiatrist" in `6hJQ53x8Pjpr9rJQ` → 1 task, `6hQF5pq7mJqHmXcQ` "Book
podiatrist appointment (foot nails)", This Month, labels book/health, no
comments. No home address in state or vault; "near me" read as Riga
(instance timezone) and named as an assumption in the comment.

Research (WebSearch + WebFetch): vc4.lv (prices and two Riga sites),
capitalclinicriga.lv (four named podologists, no prices), arsmed.lv
(Skolas 5, no prices on cenrādis index), zl.lv (Ilga Prinkalne, Čaka 50),
piearsta.lv listing for Podoloģijas akadēmija. Fetch failures reported as
such: 1slimnica.lv 403, piearsta.lv 403, arstnieciskaispedikirs.lv has no
phone or prices.

Write: `add-comments` on `6hQF5pq7mJqHmXcQ` → `6hV249G5VvR5V3HQ` (postedAt
2026-09-09T15:00:43Z), six-entry shortlist with addresses, phones,
booking route and vc4 prices. Read back via `find-comments commentId`.
No section move, no due date, no completion. Watermarks unchanged.

## 2026-09-09 — ad-hoc: podiatrist shortlist, Mārupe follow-up (chat)

Eriks, in chat: *"is there anything in Marupe?"*. WebSearch + WebFetch:
marupesdoktorats.lv podologs page (Pededzes 10, tel 20004039, four
podologists with days, price list quoted), zl.lv listing for a Piņķi
practice (Jūrmalas 14, unverified). Write: `add-comments` on
`6hQF5pq7mJqHmXcQ` → `6hV256M59ghcqcCQ` (postedAt 2026-09-09T15:06:10Z),
entries 7 and 8. Read back via `find-comments commentId`. No section move,
no due date, no completion. Watermarks unchanged.

Noted (assistant memory, not state): the question implies Eriks's home
area is Mārupe — recorded as inferred and unconfirmed.

## 2026-09-09 — ad-hoc: podiatrist availability check (chat)

Eriks, in chat: *"they're all available only in October"*. Checked the two
public booking portals in the browser, no login, read-only:

- vc4.piearsta.lv, catalogue filtered to Podologs → 3 podologists:
  Olga Gogajeva (Capital Clinic, Duntes 15A) 25.09.2026 09:00; Agnese
  Arsentjeva (same clinic) 01.10.2026 12:00; Iveta Šūmākere (VC4
  Dermatoloģijas klīnika, Skanstes 50) 12.10.2026 08:00.
- ars.eveselibaspunkts.lv, service "Ārstnieciskā pēdu aprūpe (podologs)",
  InstitutionCode 010064801 ServiceCode 48 → 4 podologists, all at ARS
  Rehabilitācija, Brīvības 39: Kuļpe 01.10 09:00, Kuhta 12.10 14:30,
  Maškanceva 29.10 09:00, Kozlovska 04.11 13:00.

So the earliest online slot in Riga is 25 Sep, not October — Eriks's
statement holds for Mārupes Doktorāts and for most but not all of the
Riga list. VC4 Pēdu centrs (K. Barona 115) and Ilga Prinkalne are not on
either portal; phone only, availability unknown, named as unknown.

Write: `add-comments` on `6hQF5pq7mJqHmXcQ` → `6hV26R5MPjPMjHWQ` (postedAt
2026-09-09T15:14:22Z). Read back via `find-comments commentId`. No
booking made (Phase 1: booking is Eriks's act), no section move, no due
date, no completion. Watermarks unchanged.

## 2026-09-10 — /start-day (full routine)

**Step 0 — orient.** Gmail: **live**, `list_labels` returned 46 labels,
`INBOX.threadsTotal` 22. Calendar: **live**, `list_calendars` returned 8
calendars, both swept ids present (`epetersons87@gmail.com`,
`family17271500024496324001@…`). Tracker: **live**, `user-info` returned
`epetersons87@gmail.com` (user 22613842). Ids not re-resolved — `_verified`
2026-09-07, 3 days old. Not a rerun: `calendar.last_scanned_date` and
`inbox.last_sweep_date` both read 2026-09-09.

Open questions read back: 2 open, **2 answered and closed**, 0 ambiguous.

- `6hRvw46QX46528XQ` Supabase — Eriks, 2026-09-09: *"yes, keep it alive"* =
  option (b). Created `6hV6j2wRWpW27hPQ` "Unpause the Supabase project
  ChallengeFinds", Backlog, size/XS, p3 (the project had already been paused
  on 9 Sep, so the title says unpause, per the assistant's own 9 Sep comment).
  The other half of option (b) — labelling thread `1a07ff008d25cd60` Reply/Do
  — was **not** performed: `get_thread` showed `label_ids` `["UNREAD"]` with
  no `INBOX`, i.e. Eriks had archived it himself, and the sweep labels inbox
  threads only. Recorded in a task comment.
- `6hRvwHmgrvg8jRHQ` vault frontmatter key — Eriks, 2026-09-09: *"I don't
  really mind. Do what you think is right. As long as it continues working and
  doesn't bite us in the future."* Decision delegated; took the stated default
  (option a): `wiki:` canonical for new files, the 15 `wiki_pages:` files in
  `raw/processed/` left untouched permanently, because that folder is declared
  never-edited and nothing reads the key. Recorded as **SETTLED 2026-09-10**
  in `procedures/step-3-ingest.md` (read back, lines 48–61) so it is not
  re-raised. Both tasks completed; `find-tasks` on `agent-waiting` in Personal
  then returned 0 — verified.

**Step 1 — inbox labelling.** Census: 22 threads read against
`INBOX.threadsTotal` 22 — the sweep count does not exceed the containing
total. 21 labelled, 1 skipped as already labelled (Revolut, Banking & Cards,
`19ecbccd34e3286c`). Bulk classification delegated to three read-only
subagents (5 threads each, no writes); the manager re-read every thread's
`label_ids` before and after each write and read all seven action-class
candidates itself in `PLAIN_TEXT`.

Labelled per class: Promotions & Ads 8, Newsletters & Learning 3,
Professional Networking 4, Receipts & Subscriptions 1, Needs-Payment 2,
Schedule Calendar 2, Reply/Do 1. **All 21 read back** in one
`search_threads in:inbox` (`THREAD_VIEW_METADATA_ONLY`) — each thread carried
exactly one taxonomy label on every message.

Ledger rows appended, then verified by grep with line numbers **before** any
archive (the 2026-09-09 lesson): promotions.csv +8 (lines 10074–10081, total
10072→10080), newsletters.csv +3 (lines 6213–6215, total 6211→6214),
receipts.csv +1 (line 532, total 530→531). **Archived-per-class equals
rows-appended per class**: Promotions 8 = 8, Newsletters 3 = 3, Receipts 1 = 1.

Archived (INBOX removed) 14 = Promotions 8 + Newsletters 3 + Receipts 1 +
Schedule Calendar 2. Read back with `search_threads in:inbox`: **8 threads
remain**, 22 − 14 = 8 ✓. Trashed: **0** — no Google Calendar notification
threads this run, so carve-out 6 did not fire.

Payment tasks created 2, both **This Week**, both `size/S`, p3, **no due
date** — neither body states one. Amounts are in PDF attachments
(`9112-0826.pdf`, `RE35718-00EBVB6801.pdf`) which the connector cannot read;
the descriptions say so rather than inventing a figure (2026-09-07 lesson).

Carve-out 7 fired once. Thread `1a085a1af2a3db83` carries **two** confirmed
Piearsta.lv bookings. The 18 Sep 10:30 transrectal USG already existed on
Eriks's calendar (`vmenfco7t7tf96qr0fb2cl7vt0`, description carrying
`ref: mail:1a085a1af2a3db83`) — matched, not duplicated. The 11 Sep 10:00
thyroid USG matched **neither** calendar and carried no cancellation, so
`create_event` on `epetersons87@gmail.com`, **no attendees**, description
`ref: mail:1a085a1af2a3db83`; `get_event` read back id
`srsprp6rp39gviuqrtuq54o7ig`, 2026-09-11 10:00–11:00 Europe/Riga, Neiromed
Plaza. End time is a 60-minute default, stated in the description. Thread
`1a08582e68045a8d` is the request-received notice for the same 11 Sep slot —
labelled, handled by the same check, archived.

Threads left unlabelled: **none**.

**Step 2 — triage.** Mail watermark at run start `1788941177000`
(2026-09-09 08:06:17Z). 20 of the 22 inbox threads were newer; the two older
were Revolut (June, already labelled) and Le-Glue (newest message
`1788888128000`, 2026-09-08 17:22). Sent sweep `in:sent after:2026/09/08`
returned 1 thread, the same Le-Glue conversation, no message above the
watermark — **no new commitments of Eriks's own**. Newest `internalDate`
actually processed: `1789019103000` (Sales Gravy, 2026-09-10 05:45:03Z).

`search_threads` truncation confirmed again on the Le-Glue thread: the
`in:inbox` result listed 5 messages ending with Lee's 14:46 reply, while
`get_thread` returned **8**, the last being Eriks's own 17:22 message. Per
`config/routing-rules.md` § Source hints › Mail, a thread where Eriks sent
last is waiting on the other side — no task, no move, no comment (the task
`6hRHJQwmr656PG6x` already carries the 9 Sep comment).

Calendar swept over 10–19 Sep on both ids (wider than the 7-day window, to
cover the 18 Sep event's own date per `config/sources/calendar.md`). Eriks's
own calendar: 1 event in the window. Family: 14. Advertising events: 0.
Invitations with `needsAction`: 0. Two cross-calendar overlaps surfaced, not
tasked.

Tasks created this run: 4 (Supabase unpause; 2 payment; 1 `[Needs Eriks]`),
plus the daily review task `6hV6p5RjmV4R736x` in This Week, due today.
Board moves: **0** — no new source evidence since the watermark on any open
task, so no section was touched.

**§ 3b closed payment tasks (carve-out 5).** `find-completed-tasks` over the
last 60 days returned 20; 4 begin `Pay `, of which 2 carry a `ref: mail:`
line — Elektrum (`mail:1a080f5ae9698874`) and Bite (`mail:1a072afdab0b973b`).
`search_threads label:"Action Required/Needs-Payment" in:anywhere` returned 4
threads and **neither of those two** is among them, so both were already
swapped on earlier runs. **In class this run: 0. Writes: 0.**

**Registry/diagnostic note.** `search_threads` with `label:<label_id>` returned
`{}` for `Label_302269771500551203` while the same threads plainly carried that
id (verified in the `in:inbox` read-back), and `label:"Action Required/Needs-Payment"`
returned them correctly. → **Query labels by display name, not by label id;
a label-id query returning `{}` is not evidence of absence.** Added to
`lessons-learned.md`.

**Two Needs-Payment threads outside the sweep**, found by that same query and
reported, not acted on (both below the watermark and neither in the inbox, so
out of scope for this run): `1a064a1a38308887` Arlo, 3 Sep, "€12.99 payment …
was unsuccessful again", carries Needs-Payment, archived, **no task exists**;
and `19ff87ae98e1ca85` Google AI Studio billing change with a **14 Sep 2026**
deadline, carries Needs-Payment, in **TRASH**.

**Step 3 — vault ingest.** Nothing durable from this run's sources: promotions,
newsletters, receipts and platform notifications are never snapshotted, the two
utility invoices are bills, and the medical bookings are calendar items the
calendar now holds. **0 snapshots written, 0 pages touched.**
`vault.mail_snapshot.last_internaldate_ms` therefore does not move.

`../My Brain/raw/` holds **3 pending clippings** of Eriks's own (created 6 and
8 Sep), and the vault's `log.md` ends at 2026-09-04 — before this instance
existed, so every ingest to date was Eriks's own vault session. Whether the
daily run should work that backlog is genuinely undecided in
`procedures/step-3-ingest.md`, so it is now `[Needs Eriks]` task
`6hV6p3RqhfgHV5QQ`, default *the run ingests only its own snapshots and
reports the count*.

**Step 4 — brief.** Delivered in chat and archived to `briefs/2026-09-10.md`.

**Defaults applied this run:** the 60-minute event end for the 11 Sep booking
(mail states only a start); the vault-key default (option a) under Eriks's
delegation; the Newsletters-vs-Promotions tie-break on 3 borderline threads
(Substack/Omarchy, AI Automation Society, NEXT.io — all editorial-shaped mail
whose payload is a paid event or product).

**Anomalies:** the two out-of-scope Needs-Payment threads above; the label-id
query defect; Skool classified Professional Networking rather than Social
Media (it is a professional-community notifier, but the alternative is
defensible and Eriks may re-label).

### 2026-09-10 — addendum, after the brief (Eriks's answers in chat)

Three follow-ups put to Eriks with the brief; two settled, one open.

1. **Arlo €12.99 failed payment (`mail:1a064a1a38308887`)** — Eriks: *"If it's
   taken from Telegram - no, no need."* The condition is **false**: the item
   came from Gmail (`failed-payments@arlo.com`, 3 Sep, archived, still carrying
   `Needs-Payment`), and this instance has **no Telegram source at all** —
   Telegram belongs to the separate work instance. A conditional answer whose
   condition does not hold is **not an answer**, so nothing was created and the
   question was put back to Eriks with the provenance stated. **No task. Still
   open.**
2. **Google AI Studio billing, 14 Sep deadline (`mail:19ff87ae98e1ca85`)** —
   Eriks: *"Ignore it."* No task created. The thread is in TRASH and below the
   watermark, so no future sweep will re-surface it; no rule change needed.
   Decision recorded here only.
3. **18 Sep transrectal USG prep** — Eriks: *"Is that for the doctor
   appointment? I'd wait to see if they contact me and then give them a call
   next week if they don't."* Created `6hV6vFxQhGg9887Q` "Call Republikas
   laukuma klīnika about the 18 Sep USG if they haven't been in touch" —
   **This Month**, due **2026-09-14**, size/XS, p3,
   `ref: chat:2026-09-10-usg-prep-call`, clinic tel. 28002363 in the body.
   Section follows the 2026-09-09 precedent for a dated watch-then-act task
   (the Le-Glue refund watch, `6hRxmG7HFGvq6Pwx`, which Eriks placed in This
   Month himself). **The due date is the assistant's, not Eriks's** — his word
   was "next week"; Monday was chosen because it leaves room to buy the
   suppository before Friday, and the task description says so explicitly.
   Verified by `find-tasks` on the This Month section: 5 tasks, the new one
   present with `dueDate 2026-09-14` and `sectionId 6hJQ55cfhvj9Hq6Q`.
   **No separate Microlax purchase task** — Eriks's answer covers the prep
   through the call, and the detail already sits on calendar event
   `vmenfco7t7tf96qr0fb2cl7vt0`.
