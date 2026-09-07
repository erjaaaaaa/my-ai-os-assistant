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
