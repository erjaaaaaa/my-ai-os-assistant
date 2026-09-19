# Plan — run `/start-day` as a Claude Code cloud routine

Date: 2026-09-19. Decision: Eriks, in chat — *"let's go with the option 1.
Make the whole step by step plan and let's implement it."* Option 1 of the
ten offered on 2026-09-19: an Anthropic-hosted Claude Code cloud routine
running this repository on a schedule with the claude.ai connectors.

Status legend: **done** (verified by a read), **waiting on Eriks**, **open**.

| # | Step | Owner | Status |
|---|---|---|---|
| 1 | Write `procedures/cloud-run.md` (unattended-run rules, Step 3 deferral default, delivery = commit) | assistant | done |
| 2 | Step 0 § 0 git pull; Step 0 § 5 Step 3 exemption from the same-day no-op | assistant | done |
| 3 | Step 5 § 3 push after commit, rebase once, never force | assistant | done |
| 4 | Skills: `INSTANCE_ROOT` resolves to the repo root, not the laptop path | assistant | done |
| 5 | `AGENTS.md` § The routine: record the cloud routine and supersede "nothing is scheduled" | assistant | done |
| 6 | Add `origin` remote `git@github.com:erjaaaaaa/my-ai-os-assistant.git` | **Eriks** (the assistant's attempt was blocked by the desktop app's permission classifier as a possible data exfiltration; the SSH key on the laptop already authenticates as `erjaaaaaa`) | done 2026-09-20 — `git remote -v` shows origin |
| 7 | Create the private repository `my-ai-os-assistant` on GitHub (account `erjaaaaaa`) | **Eriks** | done 2026-09-20 |
| 8 | First push of `main` to origin; verify with `git ls-remote` | **Eriks** (same block as 6), assistant verifies | done 2026-09-20 — `git ls-remote origin` returns `main` at `541ae2c`, local tracks `origin/main` |
| 9 | Give the Claude GitHub App **write** access to the repository: install it on `my-ai-os-assistant` at https://github.com/apps/claude/installations/select_target (or reconnect GitHub from claude.ai connector settings). The creation-time check only proved **read** access — the assistant marked this done on 2026-09-20 and was wrong; the first run's `git push` returned 403 | **Eriks** | **waiting on Eriks** (reopened 2026-09-20) |
| 10 | Create the routine (disabled): repo, model `claude-opus-5`, cron `0 4 * * *` UTC, self-contained prompt. First attempt on 2026-09-19 returned HTTP 403 "You don't have access to a repository this routine uses" — creation needs 7 and 9 first | assistant, after 7 and 9 | done 2026-09-20 — `trig_01U69oWX9gmDUCgn5F5i4g22`, recorded in `state/state.json` `cloud.routine_id` |
| 11 | Attach Gmail, Google Calendar and Todoist connectors to the routine — and **only** those three. Creation attached every claude.ai connector by default (24, including Slack, Resend, Notion, Google Drive), which the security boundary forbids; narrowed by API to the three, each with a permitted-tool list mirroring `config/tools.md` (no send, reply, forward, spam, delete, draft) | assistant | done 2026-09-20 — verified by reading the routine back |
| 12 | Enable the routine and fire one run by hand; read its run log; confirm the brief was committed and pushed | assistant, after 9 and 11 | **partly done 2026-09-20** — enabled; manual run `cse_01WLbEPgazXGPVFJdavv5kNW` completed the routine (all three connectors live; 44 inbox threads read, 33 labelled, 26 archived, 25 ledger rows, 3 Todoist tasks, Step 3 deferred, brief written) but **the push failed with 403**, so the brief, ledger rows, run-log entry and watermarks exist only in that container (commits `a6a2d72`, `3fe1802`, `a3b2583`). Recovery in 12b |
| 12b | Recover the stranded record: after step 9, open the run session (`https://claude.ai/code/session_01WLbEPgazXGPVFJdavv5kNW`) and tell it to push; if the container is gone, the assistant reconstructs the 25 ledger rows from the archived thread ids in the run log, advances the watermarks to what the run recorded, and re-archives the brief from the run's final message | **Eriks**, then assistant | open |
| 12c | Step 0 now proves write access with `git push --dry-run` before any external write, and stops if refused (procedure edited 2026-09-20) | assistant | done |
| 13 | First laptop run after the cloud run: pull, confirm Steps 1–2 no-op and Step 3 catches up | assistant | open |
| 14 | Decide the vault: keep Step 3 on the laptop (default) or push the vault to a private repo and run Step 3 in the cloud | **Eriks** | open — default in force |
| 15 | Decide delivery: commit only (current) or a push channel, which needs a written widening of the security boundary | **Eriks** | open — default in force |
| 16 | Late October: retune the cron if 06:00 Riga in winter is too early | Eriks or assistant | open |

## Known costs, accepted by choosing this option

- The instance (state, logs, briefs, ledgers) is copied to GitHub. The
  ledgers contain newsletter unsubscribe links with signed tokens; those are
  mailing-list tokens, not credentials, and the repository is private.
- Cron is UTC and does not follow Riga's DST.
- The routine bills against the Claude subscription's usage, on Opus.
- A wrong disposition in an unattended run is caught only when Eriks reads
  the brief — the same property Phase 1 already has, without a live chat to
  catch it sooner.
