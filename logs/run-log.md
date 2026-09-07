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
