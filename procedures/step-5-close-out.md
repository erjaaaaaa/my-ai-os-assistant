# Step 5 — Close out

Read at the start of this step.

1. **Append to `logs/run-log.md`** — a dated entry, in this order: the orient
   block from Step 0 (per-source status with totals); per-source counts
   (read / new / per bucket / low-confidence); every write with its
   verification result; open-question counts; vault snapshots and pages; any
   default applied; any anomaly. Narrative goes here, never into state.
2. **Advance watermarks in `state/state.json`** — only now, and only these:
   - `mail.last_internaldate_ms` → the `internalDate` of the newest message
     **actually processed**. Never the clock. **Never if Gmail's control query
     failed** or the source was not configured.
   - `calendar.last_scanned_date` → today, **only if** `list_calendars`
     populated this run.
   - `vault.mail_snapshot.last_internaldate_ms` → the newest message
     snapshotted, if Step 3 ran.
   - `sources.gmail.inbox.last_sweep_date` → today, only if Step 1 ran against
     a populated control.
   - `digest.last_run_date` → today, only if the digest was produced.
   Re-read the file after writing and confirm the values.
3. **Commit.** `git add -A && git commit -m "run YYYY-MM-DD"` in the instance —
   this includes `ledgers/`.
   Backups are commits, not copies. The vault is not a git repository and is
   not committed from here.
   **ADDED 2026-09-19 — then push.** When an `origin` remote exists:
   `git push origin main`. If the push is rejected, `git pull --rebase
   origin main` once and push again. If it still fails, append the failure
   to the run-log entry, commit that, and end the run with the failure as
   the first line of the final message — never force-push, never retry in a
   loop. The pushed repository is the source of truth from this date
   (`procedures/cloud-run.md` § 2.2).
4. **Registry drift.** If any tool used this run is missing from
   `config/tools.md`, or a listed one was gone, fix the registry now and say
   so in the log entry.
5. Report in chat, after the brief: counts per step, writes with verification,
   anything that failed, and confirmation that the brief file was written.
