# Step 4 — The Personal brief

Read at the start of this step. Compose from the outputs of Steps 0–3; read
nothing new except the tracker where a count is needed.

## Sections, in order

1. **Heartbeat.** One line per source — Gmail, Calendar — saying **live**,
   **outage** or **not configured**, with the containing total and the sweep
   count (e.g. "Gmail: live, 24 inbox threads, 3 new since watermark"). The
   tracker's status. If a source is not configured, say so every time; that
   line is how it does not stay unconnected for a month.
2. **Inbox** — threads read against the inbox total; labelled per class;
   skipped as already labelled; archived per class; threads left unlabelled,
   listed by subject; "ledger not migrated — N waiting" while that holds.
3. **Payment tasks** — created this run, title and due date.
4. **Calendar events created** (CHANGED 2026-09-09, carve-out 7) — events the run created from mail, each with title, date, time, source thread and event id; and Google Calendar notifications trashed (carve-out 6), as a count with subjects. **Calendar proposals** survive only for the unsure case, asked as a `[Needs Eriks]` question. Superseded text: bookings found in mail and on neither calendar:
   title, date, time, source. "Say yes naming the item and I'll add it (no
   attendees)". Default: not created.
5. **Receipts logged** — count and total per currency, if any.
6. **Today and the next 7 days** — from Eriks's own calendar: appointments,
   each with a prep flag where a task was created; conflicts. Real-person
   events only (ADDED 2026-09-08, rule in `config/routing-rules.md` § Source
   hints › Calendar); advertising events are a count in Anomalies.
6a. **Standing reminders** — ADDED 2026-09-08 by Eriks (*"remind me
   about it in the briefs"*): one line per active entry in
   `config/routing-rules.md` § Standing reminders whose start ≤ run date ≤
   end, e.g. "Vitamin D (8000DV) today — course runs to 2026-12-08". Omit
   the section when no entry is active. The line is a reminder only; the
   task in Todoist is the commitment and is not re-created here.
7. **Family calendar** — everything on it this week, as a list. Nothing here
   is a commitment of Eriks's unless a task was created for it.
8. **Invitations awaiting a response** — named, no task.
9. **New tasks** — title, bucket, ref, section. Zero is stated as zero.
10. **Board movement** — moves made, with the evidence line; "possibly done"
   flags where a move was withheld.
11. **Waiting on others** — threads where Eriks sent the last message, without
   a task.
12. **Open questions** — `[Needs Eriks]`: the count and a pointer to the
   Waiting / Blocked section. Never the question bodies.
13. **Vault** — snapshots written to `raw/`, pages created or updated, or
   "nothing durable this run".
14. **Anomalies** — anything the classification could not decide, any count
    that looked odd, any default that was applied.

When a digest was produced this run, add one line pointing at its file.

Keep it to what changed. A quiet day is a short brief that says why it is
quiet, source by source.

## Delivery

The brief is the **closing message in chat** — not a file the reader has to
open. It is also archived to `briefs/YYYY-MM-DD.md` (the run date, Europe/
Riga). Verify the file exists after writing it.

Because the brief asks real things — possibly-done flags, open questions,
sizing prompts — offer to work through them after delivering it. **Never
infer an answer from silence**: every stated default holds until Eriks
answers in words, and a default is never recorded as a decision.
