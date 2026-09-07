---
id: gmail
configured: true
status_reason: connector live; control query populated 2026-09-07
mailbox: epetersons87@gmail.com
watermark_key: mail.last_internaldate_ms
watermark_unit: epoch_ms
reference_format: "mail:<thread_id>"
control_query: list_labels
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

**Read-only for triage.** The only permitted write is a draft, written when
Eriks asks for one. Never send, reply, forward, archive, trash, label, or mark
anything read or spam.

Before recording an absence, run the control query below. An unreadable
mailbox is an **outage**, reported as one, and its watermark does not advance.

## Tools

Match on the stable tool name. Connector instance ids appear in tool names as
`mcp__<instance-id>__<tool>` and change on reconnection — never hardcode one.

- `search_threads` — window queries (`in:inbox`, `in:sent`, `after:`). Read.
- `get_thread`, `get_message` — full thread and message bodies. Read. Use
  `messageFormat: PLAIN_TEXT`.
- `list_labels` — the control query. Read.
- `create_draft`, `update_draft` — drafts only, only on Eriks's request.
  Write, never sent.

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

**A draft, when Eriks asks for one. Nothing else.** A draft does not leave the
mailbox, so it does not touch the property the security boundary protects. This
permits the mechanism, not the initiative: the assistant deciding unprompted
that a thread deserves a reply is gated (see § Phase gates in `AGENTS.md`).

No label is written in this phase, including creating one. The mailbox's label
tree is Eriks's own, and the assistant's own namespace has not been designed
yet — a deferred decision recorded in `logs/run-log.md`, not a permission.

## Forbidden

`send_message`, `reply`, `forward`, sending or scheduling a draft,
`label_thread`, `unlabel_thread`, `label_message`, `unlabel_message`,
`update_message_labels`, `create_label`, `update_label`, `delete_label`,
archiving, trashing, marking spam, marking read. Sending is Eriks's act and
theirs alone.

## Verified defects (carried from the system this adapter was generalised from)

Both are the same trap: the tool answers, the answer is incomplete, and the
incompleteness is silent.

- **`search_threads` truncates a thread's message list.** One thread returned 5
  messages with a newest date three weeks stale, while `get_thread` on the same
  id returned 9, newest that morning. No error, no truncation flag. **Call
  `get_thread` whenever the newest-message timestamp matters** — which is every
  watermark comparison.
- **Negative label filters do not work at thread level.** `in:inbox -label:X`
  returns threads carrying X anyway, because the query matches a thread if
  *any* message in it lacks the label. Verify labels by reading each message's
  `label_ids`, never by filtering them out in the query.
