# Cloud run — the routine without the laptop

ADDED 2026-09-19 on Eriks's decision in chat, choosing option 1 of the ten
offered (a Claude Code cloud routine): *"let's go with the option 1. Make
the whole step by step plan and let's implement it."* The plan and its
status live in `plans/2026-09-19-cloud-routine.md`.

Read at Step 0 whenever the run is unattended. Everything in `AGENTS.md`
still applies; this file only says what an unattended run does differently.
Where this file and `AGENTS.md` disagree, `AGENTS.md` wins and the
disagreement is a defect to fix here.

## 1. How a run knows it is unattended

A run is a **cloud run** when the routine prompt says so, or when the
working directory is a git clone with an `origin` remote and no
`../My Brain/` folder beside it. Record `run context: cloud` (or `chat`) in
the orient block at Step 0 and in the run-log entry at Step 5.

## 2. What differs from a chat run

1. **No chat.** Eriks is not present and never sees the session live.
   Nothing is asked in the session output. Every decision that is his
   becomes a `[Needs Eriks]` task with a stated default, exactly as
   `AGENTS.md` § `[Needs Eriks]` describes; the answer channel stays the
   task's comments, read back at the next Step 0.
2. **Git bookends the run.** Step 0 § 0 pulls; Step 5 § 3 pushes. The
   pushed repository is the source of truth for state, logs, briefs and
   ledgers from 2026-09-19 on. iCloud on the laptop is a clone, not the
   master: a laptop run pulls first, like any other clone.
3. **Step 3 is deferred — default in force until Eriks decides.** The vault
   `../My Brain/` is not in the clone, so the cloud run cannot reach it.
   The run records *"Step 3 deferred: vault not reachable from this runner"*
   in the log, leaves `vault.mail_snapshot.last_internaldate_ms` where it
   is, and names the deferral in the brief. The next run that can reach the
   vault (a laptop run) does Step 3 from that watermark and clears `raw/`;
   Step 0 § 5 exempts Step 3 from the same-day no-op for exactly this case.
   **The alternative, which only Eriks can choose:** push the vault to a
   private GitHub repository, add it as a second source of the routine, run
   Step 3 in the cloud, and let Obsidian pull it through the Git plugin.
   The cost is a copy of the vault — family, health, money — on GitHub's
   disks. Not chosen yet; silence keeps the deferral.
   **PROPOSED 2026-09-20 — Eriks asked for the alternative in chat:** *"I'm
   still not clear how the vault ingest should work. Can we move it online
   and then the local obsidian catches up with whatever was advanced
   online?"* Read as a request for the design and a yes in principle, **not
   yet as the per-item yes to copying the vault to GitHub** — that cost was
   named to him as the thing only he can choose, and the question form
   leaves it open. The design and its steps are in
   `plans/2026-09-19-cloud-routine.md` § Vault online (steps 14a–14g). The
   deferral above stays in force until the vault repository exists, the
   Claude GitHub App can write to it, and § 5 below is marked in force.
   **DECIDED 2026-09-20 by Eriks — the vault stays local:** *"Ok, let's
   keep the vault local."* The proposal above is parked, not withdrawn (the
   design stays in the plan). Step 3 runs only on the laptop, in a
   `/start-day` Eriks starts themself, from `vault.mail_snapshot.
   last_internaldate_ms`; the search it runs is now written down in
   `procedures/step-3-ingest.md` § Finding mail to snapshot.
4. **Consent, phase gates and the security boundary are unchanged.** The
   cloud runner has the write permissions of `AGENTS.md` § Security
   boundary and nothing more. Running unattended widens nothing.
5. **Delivery is the commit.** The brief is archived to `briefs/` and
   pushed; the routine's run page at `https://claude.ai/code/routines`
   shows the same text as the session's final message.
   **WITHDRAWN IN PRACTICE 2026-09-20 by Eriks — no cloud brief.** *"remove
   the daily brief from the cloud run. I will just run /start-day myself
   here on the laptop whenever I'm ready."* The daily routine is
   **disabled**, not deleted; the brief is a laptop deliverable again, in
   chat and in `briefs/`. This paragraph and § 2.3 describe what a cloud
   `/start-day` does *if the routine is ever re-enabled*. **No message is
   sent on any channel** — Telegram, Slack or mail delivery would be a
   widening of the security boundary that Eriks has not written.
   **Observed 2026-09-20:** the scheduled 04:08 UTC run
   (`cse_01DAek8HnkjSoCwLFd5SGMx6`), which stopped at the write-access
   gate, nevertheless called `PushNotification` with its summary. That is
   a message to Eriks's own device, not to another person, but it is a
   tool outside `config/tools.md` used without a written yes. Both routine
   prompts now name it as forbidden explicitly; the run is recorded in the
   run log as an anomaly, not adopted as precedent.
6. **Overlap.** A laptop `/start-day` on a day the cloud run already
   completed is a rerun: Steps 1 and 2 no-op from the watermarks, Step 3
   catches up, the brief says so. A laptop run must pull before Step 0 § 1
   and must not start while a cloud run is in progress (check the routine's
   run page if in doubt); a push rejected at Step 5 is handled there, never
   forced.
7. **A failed push is reported, never hidden.** If Step 5 cannot push after
   one rebase attempt, the run ends with the failure as the first line of
   its final message. The external writes it made (labels, tasks, events)
   are real; the state that records them is only in that session. The next
   run starts from the last pushed watermarks and re-reads the same items —
   safe because Step 1 labels only unlabelled threads and Step 2 dedupes on
   `ref: mail:<thread_id>` — and it notes the gap in its log.
   **CORRECTED 2026-09-20 — "safe" was wrong for ledger rows.** Threads
   archived under carve-out 4 leave the inbox sweep for good, so the ledger
   rows written for them in a run whose push failed are lost with the
   container; the first cloud run (`cse_01WLbEPgazXGPVFJdavv5kNW`) lost 25
   this way. Two things follow. Step 0 § 0 now proves write access with
   `git push --dry-run origin main` before any external write and stops the
   run if refused. And when a push does fail after that gate, recovery is:
   grant access, open the run session and tell it to push; if the container
   is gone, reconstruct the ledger rows from the thread ids the run log
   quotes, then advance the watermarks to what the run recorded.

## 3. Registration

- **Repository:** `git@github.com:erjaaaaaa/my-ai-os-assistant` (private).
- **Routine (daily):** Claude Code cloud routine, environment Default,
  model `claude-opus-5`, cron `0 4 * * *` UTC — 07:00 Riga in summer, 06:00
  in winter (cron does not follow DST; retune in late October if it
  matters). **CHANGED 2026-09-20 by Eriks:** *"The daily brief can then be
  delivered once a day at 9 am (moved from 7 am right now)."* Cron is now
  `0 6 * * *` UTC — 09:00 Riga in summer, **08:00 in winter** after the
  clocks change on 2026-10-25; retune to `0 7 * * *` then if 09:00 is what
  matters. Superseded text: `0 4 * * *`.
  **DISABLED 2026-09-20 by Eriks:** *"remove the daily brief from the
  cloud run. I will just run /start-day myself here on the laptop whenever
  I'm ready."* Read back `enabled: false`; the cron and connectors are left
  in place so it can be re-enabled by one update if he changes his mind.
  `/start-day` is a laptop, on-demand command from this date.
- **Routine (hourly):** ADDED 2026-09-20 by Eriks: *"I want the e-mail
  sweep labelling to run every hour."* A second routine,
  `trig_01X5cu18Ba6kNcWbaAABCGR8`, same environment, model and three
  connectors, cron `0 0-5,7-23 * * *` UTC — every hour **except 06:00 UTC**,
  when the daily run does Step 1 itself. It runs the `/inbox` skill in the
  hourly form of § 4 below. **CHANGED 2026-09-20, same decision as above:**
  with the daily routine disabled there is no hour to skip; requested as
  `0 * * * *`, which the server stored and read back as `59 * * * *` (fires
  at :59 every hour). Superseded text: "except 06:00 UTC". Id in `state/state.json` under
  `cloud.hourly_routine_id`.
- **Connectors attached to the routine:** Gmail, Google Calendar, Todoist —
  the same three claude.ai connectors `config/tools.md` names, each with a
  permitted-tool list mirroring that registry. CORRECTED 2026-09-20: set by
  API, not in the UI — creation attached every claude.ai connector by
  default (24), and the update narrowed it to three; re-check the list
  whenever the routine is edited, because that default may reapply.
- **Also present in the cloud environment, not attached by us:** a GitHub
  MCP (`mcp__github__*`) and a `PushNotification` tool that sends a note to
  Eriks's own Claude mobile app. Neither is in `config/tools.md`. The GitHub
  MCP is not used. The push is a message to Eriks himself, not to another
  person, so it does not touch the property the security boundary protects
  — but it is not adopted as delivery until Eriks says so in writing.
- **Routine ids** are recorded in `state/state.json` under
  `cloud.routine_id` (daily) and `cloud.hourly_routine_id` (hourly) with a
  `_verified` date, like every other id.
- **Debugging a run:** the routine's run list and run log, read through
  the desktop session's `RemoteTrigger` tool (`list_runs`, `get_run_log`),
  or the run page in the browser.

## 4. The hourly form — `/inbox` every hour

ADDED 2026-09-20 by Eriks: *"I want the e-mail sweep labelling to run every
hour."* The hourly routine runs Step 0 and Step 1 only, then closes out.
Everything in §§ 1–2 applies; these are the differences:

1. **Scope.** Step 0 in full (pull, write-access gate, governing files,
   state, health checks, open-question answers — § 4 of Step 0 runs every
   hour, so an answer Eriks posts is applied within the hour). Then Step 1
   in full, including its post-actions: payment tasks, ledger rows,
   carve-out 4 archives, carve-out 6 trashes, carve-out 7 event creation.
   **Not** Step 2, 3 or 4, and no brief; the daily run owns those.
2. **The review task** (`procedures/step-1-inbox.md` § 5): an hourly run
   that labelled nothing leaves it alone. One that labelled something
   creates the day's task if it does not exist, otherwise posts **one**
   comment with that run's counts. Twenty-four comments a day is the
   ceiling, not the norm.
3. **Close-out** (`procedures/step-5-close-out.md` § 1 hourly form): a run
   that labelled nothing appends **one line** to the run log; a run that
   wrote anything appends a full entry. Watermarks advance as usual
   (`mail.last_internaldate_ms`, `sources.gmail.inbox.last_sweep_date`).
   Commit message `inbox YYYY-MM-DD HH:MM UTC`. Push, rebase once, never
   force.
4. **Overlap.** The hourly cron skips 06:00 UTC so it cannot collide with
   the daily run's start; a run that overruns into the next hour meets the
   next one only at the push, where the rebase-once rule applies. A laptop
   `/inbox` or `/start-day` pulls first like any clone.
   **CHANGED 2026-09-20:** the daily cloud run is disabled, so the only
   overlap left is with a laptop `/start-day`, which pulls at Step 0 and
   rebases once at Step 5 like any clone; a laptop run's Step 1 will
   usually find nothing unlabelled, which is the hourly sweep working.
5. **Same-day idempotency is by watermark and by label, not by date.**
   Step 0 § 5's date check is for Steps 2–3; Step 1 never no-ops on the
   date alone — it reads the inbox and skips threads already labelled.
6. **Cost, named not hidden:** up to 23 Opus runs a day on top of the
   daily one. A cheaper model for the hourly sweep is Eriks's lever, not
   the assistant's — the label decides whether a thread leaves the inbox,
   so the model choice is a quality decision. Recorded as an open point in
   the plan.
