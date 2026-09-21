---
id: gmail
configured: true
status_reason: connector live; control query populated 2026-09-07
mailbox: epetersons87@gmail.com
watermark_key: mail.last_internaldate_ms
watermark_unit: epoch_ms
reference_format: "mail:<thread_id>"
control_query: list_labels
labels: sources.gmail.labels in state/state.json (13 ids)
labels_verified: 2026-09-07
---

# Mail — epetersons87@gmail.com

**Mail** — inbox threads with a message newer than `mail.last_internaldate_ms`,
an **epoch-millisecond** watermark compared against each message's own
millisecond timestamp. A bare date cannot sequence two messages that arrived on
the same day, so a same-day second run would either re-read everything or skip
real items. Advance it to the timestamp of the **newest message actually
processed**, never to the clock at the end of the run.

Read the **full thread** before comparing timestamps. A search result's message
list can be silently incomplete, so the newest message in a thread is only
reliable when the thread itself was read. For the same reason, verify a thread's
existing labels by reading them off the messages — a label filter in the query
cannot be trusted to exclude what it claims to.

Scope, and anything excluded wholesale, is in `config/routing-rules.md`
§ Source-specific notes — edit that file to change it, never this adapter.

**Writes are narrow and listed below.** Step 1 adds one of thirteen labels to
an unlabelled inbox thread and archives the four carve-out classes; a draft is
written when Eriks asks for one. Never send, reply, forward, or mark anything read or spam; trash only under allowed-write 5 (NARROWED 2026-09-09; superseded text: "Never send, reply, forward, trash, or mark anything read or spam.").

Before recording an absence, run the control query below. An unreadable
mailbox is an **outage**, reported as one, and its watermark does not advance.

## Tools

Match on the stable tool name. Connector instance ids appear in tool names as
`mcp__<instance-id>__<tool>` and change on reconnection — never hardcode one.

- `search_threads` — window queries (`in:inbox`, `in:sent`, `after:`). Read.
- `get_thread`, `get_message` — full thread and message bodies. Read. Use
  `messageFormat: PLAIN_TEXT`.
- `list_labels` — the control query. Read.
- `label_thread` — add exactly one of the thirteen label ids (from state) to
  an inbox thread carrying none of them. **Write, additive only.**
- `unlabel_thread` with `["INBOX"]` — archive, **only** for the four carve-out
  classes after their post-action was verified. **Write.** Never any other
  label id.
- `create_draft`, `update_draft` — drafts only, only on Eriks's request. Write, never sent.
- `trash_thread` — **only** a Google Calendar notification thread under carve-out 6 (ADDED 2026-09-09). Write. Read back with `search_threads … in:anywhere` and `includeTrash: true` (`get_thread` refuses a trashed thread).

## Control query

`list_labels` on the mailbox, scoped to nothing. A populated result is the
mailbox's **full** label list, including Eriks's own labels and the system
ones. It is independent of any time window, so it comes back populated on a
live connector whatever the sweep found. On 2026-09-07 it returned 55 labels.

Pair it with the **containing total** whenever reporting counts: the `INBOX`
entry's `threadsTotal` next to the number of threads the sweep read, plus an
explicit statement that the sweep count does not exceed it.

Until `list_labels` returns populated, an empty sweep is an **outage**, is
reported as one, and the watermark does not advance.

## Watermark

Key `mail.last_internaldate_ms` in `state/state.json`, unit epoch milliseconds.
Compare each message's `internalDate` (an epoch-millisecond **string** exposed
beside the ISO date on every message in `search_threads`, `get_thread` and
`get_message` results — read it directly, never derive it from the ISO string)
against the stored value. Advance to the `internalDate` of the newest message
actually processed.

## Allowed writes

1. **One label per thread, from the thirteen** in `state/state.json`
   (`sources.gmail.labels`), on an inbox thread that carries none of them.
   These are Eriks's own labels, by Eriks's choice on 2026-09-07 — the sweep
   replicates the automation that used them before. Additive only: a label is
   never removed, renamed or created, and a thread already labelled is never
   relabelled by a run (Eriks's correction wins).
2. **Archive — removing `INBOX` — for exactly four classes**: Newsletters &
   Learning, Promotions & Ads, Receipts & Subscriptions once its ledger row
   is verified, Schedule Calendar once handled. The carve-out and its
   boundaries are in `AGENTS.md` § Phase gates.
3. **A draft, when Eriks asks for one.** A draft does not leave the mailbox,
   so it does not touch the property the security boundary protects. This
   permits the mechanism, not the initiative: proactive drafting is gated.
4. **The closed-payment swap** (WIDENED 2026-09-07, Eriks; carve-out 5 in
   `AGENTS.md` § Phase gates): on a thread still carrying `Needs-Payment`
   whose `Pay …` task with a `ref: mail:` line Eriks has completed —
   `label_thread` with `Paid` (id `paid_label_id` in state), then
   `unlabel_thread` with `Needs-Payment`, then `unlabel_thread` with `INBOX`.
   Each read back. The only label removal and the only non-taxonomy label this adapter ever writes.
5. **Trash, for exactly one class** (WIDENED 2026-09-09, Eriks; carve-out 6 in `AGENTS.md` § Phase gates): a Google Calendar notification thread — New/Updated/Cancelled event, Invitation, Accepted/Declined, daily agenda, and (WIDENED 2026-09-21, Eriks, *"a)"* on task `6hWmrXJ7Cg6hF9Wx`) "Notification:" reminders — after the existence check, via `trash_thread`. Eriks: *"The notifications from google should be ignored (those are usually about the event creation or deletion) so those can be deleted immediatlly."* Nothing else is ever trashed.

6. **A per-item label removal Eriks asks for in chat** (WIDENED 2026-09-10,
   Eriks). The class is exactly: Eriks names the thread **and** the label, in
   chat, and asks for it off. `unlabel_thread` with that one id, read back two
   ways — the thread's own `label_ids`, and the label's own
   `label:"<Display Name>"` count before and after. Both boundaries: **not** a
   class of threads, **not** an inference from a related instruction, and
   **never** the assistant deciding for itself that a label is wrong; and it
   licenses no renaming, no deletion, no archive and no trash. First use: the
   Arlo failed-payment thread `1a064a1a38308887`, Eriks: *"Arlo - remove the
   label. There is money on the account now and I simply waiting for them to
   try charge it again"* — the thread kept `Needs-Payment` from an earlier
   manual filing, the charge is expected to retry on its own, and no task was
   wanted.

7. **A per-item archive Eriks asks for in chat** (WIDENED 2026-09-11, Eriks).
   The class is exactly: Eriks names the thread, in chat, and asks for it
   archived. `unlabel_thread` with `["INBOX"]` on that one thread, read back
   via `in:inbox`. Both boundaries: **not** a class of threads and **not** an
   inference from a related instruction (the thread's taxonomy label is
   irrelevant — it stays on the thread untouched), and **never** the assistant
   deciding for itself that a thread is finished; and it licenses no label
   removal beyond `INBOX`, no trash, no spam and no mark-read. First use: the
   Le-Glue conversation `1a0813348da28051`, which carried **Reply/Do** and so
   fell outside carve-out 4 — Eriks: *"Le-Glue refund received, we can archive
   the email and close the task"*. Same authorisation channel as allowed-write
   6, and same reasoning: the act is inside Eriks's own mailbox, reversible,
   and reaches no other person.

8. **A per-item `Paid` application Eriks asks for in chat** (WIDENED
   2026-09-14, Eriks). The class is exactly: Eriks names the thread, in chat,
   and says to mark it paid. `label_thread` with `paid_label_id` on that one
   thread, read back. Both boundaries: **not** a class of threads, **not** an
   inference that a payment looks settled, and **never** the assistant deciding
   a thread is paid — only Eriks knows that; and it licenses no archive, no
   label removal and no trash, each of which needs its own instruction. First
   use: the YouTube Premium failed-payment thread `1a099e498f6dc9a7`, after
   Eriks fixed the card himself — *"I've already updated the payment details
   and it went though all fine. Can mark is as Paid."* `Needs-Payment` was
   deliberately **not** applied first, because the end state Eriks asked for is
   `Paid` and the two labels must never rest together (`config/routing-rules.md`
   § The payment task). Same authorisation channel as allowed-writes 6 and 7,
   and safer than both: adding a label is additive, mailbox-internal,
   reversible, and reaches no other person.

## Forbidden

`send_message`, `reply`, `forward`, sending or scheduling a draft,
`label_message`, `unlabel_message`, `update_message_labels` (labels are
thread-level here), `create_label`, `update_label`, `delete_label`,
`unlabel_thread` with any id other than `INBOX` or on any thread outside the
four carve-out classes (NARROWED 2026-09-07: allowed-write 4 above is the one
exception), `apply_sensitive_thread_label`, trashing (NARROWED 2026-09-09: allowed-write 5 is the one exception), marking spam, marking read. Sending is Eriks's act and theirs alone.

## Verified defects (carried from the system this adapter was generalised from)

Both are the same trap: the tool answers, the answer is incomplete, and the
incompleteness is silent.

- **`search_threads` truncates a thread's message list.** One thread returned 5
  messages with a newest date three weeks stale, while `get_thread` on the same
  id returned 9, newest that morning. No error, no truncation flag. **Call
  `get_thread` whenever the newest-message timestamp matters** — which is every
  watermark comparison.
- **A `label:<label_id>` query returns `{}` even when the label is on the
  threads.** ADDED 2026-09-10: `search_threads label:Label_302269771500551203`
  came back empty while the same run's `in:inbox` read-back showed that id on
  two threads, and `label:"Action Required/Needs-Payment" in:anywhere` returned
  four. **Query labels by display name in quotes, never by id**; an empty
  label-id query is a query defect, not an absence. Label ids remain correct
  for `label_thread` / `unlabel_thread`.
- **Negative label filters do not work at thread level.** `in:inbox -label:X`
  returns threads carrying X anyway, because the query matches a thread if
  *any* message in it lacks the label. Verify labels by reading each message's
  `label_ids`, never by filtering them out in the query.
- **`resultCountEstimate` is not a count.** ADDED 2026-09-21: `search_threads
  in:anywhere after:2026/09/07` reported `resultCountEstimate: 201` and
  returned 380 threads over eight pages. **Paginate to exhaustion and count
  what came back; the estimate is never a containing total.**
