---
id: calendar
configured: true
status_reason: connector live; control query populated 2026-09-07
calendars:
  - epetersons87@gmail.com
  - family17271500024496324001@group.calendar.google.com
window_days: 7
watermark_key: calendar.last_scanned_date
watermark_unit: iso_date
reference_format: "event:<calendar_id>/<event_id>"
control_query: list_calendars
---

# Calendar — Eriks's own calendar and the Family calendar

**Calendar** — **only `epetersons87@gmail.com` and the Family calendar** (ids
in `state/state.json`). Today plus the next seven days: meetings and
appointments needing preparation, conflicts, and invitations awaiting a
response. Read-only.

**The scope rule is the important one here, and it is not a default to be
relaxed.** The account can see more calendars than it sweeps — holiday
calendars, an event calendar, and two calendars named "Todoist" that mirror
tracker tasks. **Ignore all of them.** They are not Eriks's commitments, and
sweeping the mirrors would count every task twice. The two calendars to sweep
are the ones named above; add another only when Eriks explicitly asks, naming
it. The Family calendar is in because Eriks named it: it is where their wife
adds things Eriks should know about.

**Unanswered invitations produce no task.** Name them in the Personal brief's
calendar section only. Appointments needing preparation become tasks only when
the preparation is non-trivial — a document to bring, a form to fill, a
decision to make first; "attend" is not a task. Routine recurring series never
do (none are named yet; name them in `config/routing-rules.md`).

**The seven-day window bounds how far ahead to scan — it is not a licence to
write about a date nobody looked at.** When creating or commenting on a task
that references a specific dated event, read the calendar **for that event's
own date**, whatever the window says.

Before recording an absence, run the control query below. An unreadable
calendar is an **outage**, reported as one.

## Tools

Match on the stable tool name, never on the instance-prefixed name.

- `list_calendars` — the control query. Read.
- `list_events` — events in a date range on a named calendar. Read. Pass
  `calendarId`, `startTime`, `endTime`, `timeZone: Europe/Riga`,
  `pageSize: 250`, `orderBy: startTime`; paginate on `nextPageToken`.
- `get_event` — one event's full detail, including attendee response status.
  Read.
- `search_events` — keyword lookup. Read. Optional.

## Control query

`list_calendars` with no filter. A populated result is the **complete**
calendar list — larger than the swept set, and that is the point: it is
independent of any date window. Confirm **both** swept calendar ids appear in
it. On 2026-09-07 it returned 8 calendars including both.

Second check, when the window reads as empty: `list_events` over a **wider**
window than the sweep (the past 30 days plus the next 30) on the same
calendar. An empty week is believable when a wider window is populated; two
empty windows in a row are an outage until proved otherwise.

Until the control comes back populated, "nothing this week" is not a finding —
it is an **outage**, reported as one, and the scanned-date key does not
advance.

## Watermark

Key `calendar.last_scanned_date`, an ISO date. Not a true watermark — the
calendar is read forward over the fixed window each run, and the key records
the date last scanned so a same-day rerun no-ops. It advances only after a
populated control query.

## Allowed writes

**None autonomously. This source is read-only in every sweep.** `create_event`,
`update_event`, `delete_event` and `respond_to_event` are never called by a run
on its own judgement. A calendar write reaches other people — attendees are
notified — so it sits squarely against the property the boundary protects.

**The one exception, per item:** a calendar proposal from the inbox sweep (a
confirmed booking found in mail and matched on neither calendar) that Eriks
answers with a yes **naming that item** in chat is created with `create_event`
on `epetersons87@gmail.com` — **never with attendees**, because an invite
reaches another person. Each creation is read back, logged with Eriks's words,
and the source thread is then archived. It is never a standing permission, and
the next such write needs their yes again.
