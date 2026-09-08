# Step 1 — Inbox labelling

Read at the start of this step. Skip entirely if Step 0 recorded Gmail as an
outage or not configured. Policy — the thirteen classes, their tests, the
payment-task shape, the archive classes — lives in `config/routing-rules.md`
§ Mail label taxonomy; this file is the steps. Where they disagree the config
file wins.

The manager performs every write. Reading and proposing may be delegated
(`AGENTS.md` § Delegation model); a subagent's proposal is a claim, and the
manager re-reads the thread before labelling it.

## 1. Read the inbox

1. `search_threads` with `query: in:inbox`, `pageSize: 50`, paginated to
   exhaustion. Census: threads returned ≤ `INBOX.threadsTotal` from Step 0;
   state both numbers.
2. For every thread: `get_thread`, `messageFormat: PLAIN_TEXT`. Read
   `label_ids` off the messages. **If any message carries any of the thirteen
   label ids in `state/state.json`, skip the thread** — Eriks's or an earlier
   run's label wins and is never changed. Count skipped threads.
3. For each remaining thread, classify from the **full text** per the taxonomy.
   Unsure between an action class and anything else → leave unlabelled and
   list it for the brief; never guess an action label.

## 2. Label

`label_thread(threadId, [id])` with exactly one label id from state. Read the
thread back and confirm the id is present on its messages. Never remove a
label, never create one, never apply a label outside the thirteen.

## 3. Post-actions, per class

**Needs-Payment.** Extract vendor, amount, currency, due date, invoice or
client number from the body. Dedupe the ref against open tasks, completed
(60 days) and deleted (60 days, `find-activity`). Create the payment task per
the taxonomy's shape (`add-tasks`, then `fetch-object` to confirm the `ref:`
line). The thread stays in the inbox.

**Schedule Calendar.** Apply the sub-rule first (daily agenda, marketing
webinar, `bilesuserviss.lv`): if it fires, label **Promotions & Ads** instead
and treat as that class. Otherwise parse title, start, end (default 60
minutes), attendee. `list_events` on **both** calendar ids in state over
start − 5 min to end + 5 min, `fullText` = the title, `timeZone: Europe/Riga`.
A match (title roughly equal, times overlapping) → **handled**: archive. No
match → add to the brief's **Calendar proposals**: title, date, time, source
thread, and whether an attendee was named (which would make it a per-item
question with the default *not created*). Nothing is created in this step.
When Eriks later says yes naming an item, the manager creates it per
`config/sources/calendar.md` (no attendees), reads it back, logs it, and
archives the thread.

**Receipts & Subscriptions.** Extract final total (prefer "total", "amount
due", "grand total", "kopā", "apmaksai" over subtotals; treat the last comma
or dot as the decimal separator, strip other separators), ISO currency,
invoice number, charge date (else the message date), short vendor name. If the
amount is only in an attachment, the row still gets written with
`notes: amount not in body; attachment not readable via connector`. Append to
`ledgers/receipts.csv` unless the `messageId` is already there; re-read the
file to confirm the row; then archive.

**Newsletters & Learning / Promotions & Ads.** Append to
`ledgers/newsletters.csv` / `ledgers/promotions.csv`: received_date (message
date), from_name, from_address, source_domain, subject, threadId, messageId,
threadUrl
(`https://mail.google.com/mail/?authuser=epetersons87@gmail.com#all/<threadId>`
— CORRECTED 2026-09-07: the earlier `…/mail/u/0/#all/<threadId>` form opens
whichever Google account the browser treats as first and falls back to that
account's inbox when the thread is not there; Eriks: "open in Gmail … doesn't
carry the direct thread link, it just opens inbox"), list_id
(empty — not exposed by the connector), list_unsubscribe (`body-cue` when the
body carries unsubscribe / manage-preferences text, else empty), digested
(empty). Dedupe on messageId; re-read; then archive.

**Every other class.** Label only.

**If a ledger file does not exist**, the class is labelled, **no row is written
and the thread is not archived**; the brief says "ledger not migrated —
N threads waiting" (see `ledgers/README.md`).

## 4. Archive — the carve-out, and nothing beyond it

Archive = `unlabel_thread(threadId, ["INBOX"])`, only for the four classes in
the taxonomy, only after that class's post-action succeeded and was verified.
Read back and confirm `INBOX` is absent. Never trash, never mark spam or read,
never archive any other class.

## 5. The review task

Upsert one Todoist task for the day in Personal, **This Week** (section id
`this_week` in state) — CORRECTED 2026-09-08 by Eriks: *"When you add review
labels tasks, don't just mark it for today, but also move it THIS WEEK column,
same as with the payments."* Superseded text: "Backlog". Title `[Act] Review inbox
labels — YYYY-MM-DD`, p3, due today, description = counts per label, archived
per class, payment tasks created (titles), calendar proposals (titles), threads
left unlabelled (subjects). Search by title first; update rather than
duplicate.

## 6. Weekly digest hand-off — WITHDRAWN 2026-09-07

Withdrawn by Eriks after the first `/start-day` produced the digest:
*"Digest should be just sent once a week or on demand."* The digest is now
produced only by `/digest` (see `AGENTS.md` § Delivery). Superseded text:
"If today is on or after the most recent Friday 16:00 Europe/Riga and
`digest.last_run_date` in state is before that Friday, run
`procedures/digest.md` now, after the ledgers are updated." This step does
nothing.

## Output

For the brief and run log: inbox total, threads read, skipped (already
labelled), labelled per class, archived per class, payment tasks, calendar
proposals, ledger rows written, unlabelled threads listed with subjects, and
every write with its read-back result.
