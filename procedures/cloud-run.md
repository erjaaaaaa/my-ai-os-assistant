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
4. **Consent, phase gates and the security boundary are unchanged.** The
   cloud runner has the write permissions of `AGENTS.md` § Security
   boundary and nothing more. Running unattended widens nothing.
5. **Delivery is the commit.** The brief is archived to `briefs/` and
   pushed; the routine's run page at `https://claude.ai/code/routines`
   shows the same text as the session's final message. **No message is
   sent on any channel** — Telegram, Slack or mail delivery would be a
   widening of the security boundary that Eriks has not written.
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
- **Routine:** Claude Code cloud routine, environment Default, model
  `claude-opus-5`, cron `0 4 * * *` UTC — 07:00 Riga in summer, 06:00 in
  winter (cron does not follow DST; retune in late October if it matters).
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
- **Routine id** is recorded in `state/state.json` under `cloud.routine_id`
  with a `_verified` date, like every other id.
- **Debugging a run:** the routine's run list and run log, read through
  the desktop session's `RemoteTrigger` tool (`list_runs`, `get_run_log`),
  or the run page in the browser.
