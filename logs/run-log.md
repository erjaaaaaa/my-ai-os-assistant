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
