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
| 14 | Decide the vault: keep Step 3 on the laptop (default) or push the vault to a private repo and run Step 3 in the cloud | **Eriks** | **asked for 2026-09-20** — *"Can we move it online and then the local obsidian catches up with whatever was advanced online?"* Design in § Vault online below; the deferral stays until 14a–14d are done |
| 15 | Decide delivery: commit only (current) or a push channel, which needs a written widening of the security boundary | **Eriks** | open — default in force |
| 16 | Late October: retune the cron if 06:00 Riga in winter is too early | Eriks or assistant | open — now reads: after 2026-10-25 the daily fires 08:00 Riga; move to `0 7 * * *` UTC if 09:00 is what matters |
| 17 | Daily routine moved to 09:00 Riga — cron `0 6 * * *` UTC. Eriks 2026-09-20: *"The daily brief can then be delivered once a day at 9 am (moved from 7 am right now)."* Prompt also now names `PushNotification` and the GitHub MCP as forbidden, after the 04:08 UTC run used the former | assistant | done 2026-09-20 — routine read back: `cron_expression` `0 6 * * *`, `next_run_at` 2026-09-21T06:07Z |
| 18 | Hourly `/inbox` routine `trig_01X5cu18Ba6kNcWbaAABCGR8`, cron `0 0-5,7-23 * * *` UTC, same environment, model and three connectors, created **disabled**; procedure in `procedures/cloud-run.md` § 4. Eriks 2026-09-20: *"I want the e-mail sweep labelling to run every hour."* | assistant | done 2026-09-20 — create response lists exactly the three connections |
| 19 | Fire one hourly run by hand, read its log, then enable | assistant | open |
| 20 | Model for the hourly sweep: Opus (current, parity with the daily) or a cheaper model — up to 23 extra Opus runs a day | **Eriks** | open — default Opus |

## Vault online — design for step 14 (proposed 2026-09-20, not yet in force)

**Goal, in Eriks's words:** *"move it online and then the local obsidian
catches up with whatever was advanced online."* Mechanism: the vault becomes
a private git repository; the cloud routine clones it as a second source,
runs Step 3 there and pushes; the laptop's Obsidian pulls it back through the
community **Git** plugin (obsidian-git), so Eriks sees the ingested pages
without running anything. Web-clipper saves on the laptop go the other way by
the same plugin's auto-commit-and-push.

| # | Step | Owner | Status |
|---|---|---|---|
| 14a | Say yes in writing to the cost: a copy of the vault — family, health, money — in a private GitHub repository. Also say whether the vault stays in iCloud Drive (default — nothing moves, Obsidian on other Apple devices keeps working) or moves out of it (git becomes the only sync; avoids iCloud syncing `.git` internals, the one known trouble spot) | **Eriks** | open |
| 14b | Create the empty private repository `erjaaaaaa/my-brain` on GitHub | **Eriks** | open |
| 14c | `git init` in `../My Brain/`, `.gitignore` for `.obsidian/workspace*.json`, `.DS_Store`, `.trash/`; first commit; add `origin`; push `main`. The remote-add and push were blocked by the desktop app's classifier for the assistant repo, so these may again be Eriks's steps; the assistant prepares the tree and verifies with `git ls-remote` | assistant, then **Eriks** if blocked | open |
| 14d | Grant the Claude GitHub App write access to `my-brain` (same installation page as step 9); a cloud probe run proves it with `git push --dry-run` | **Eriks** | open |
| 14e | Add `https://github.com/erjaaaaaa/my-brain` as a second `git_repository` source of the daily routine; a probe run reports where the clone lands (expected beside the instance, `/home/user/my-brain`); record the candidates in `state/state.json` `vault.paths` and make Step 3 resolve the vault by finding a folder among them that holds `AGENTS.md`, `raw/` and `wiki/` | assistant | open |
| 14f | Procedures: `cloud-run.md` § 1 drops the "no `../My Brain/`" heuristic (the prompt alone says cloud); § 2.3 deferral WITHDRAWN; Step 0 § 0 pulls and dry-run-pushes the vault too; Step 5 § 3 commits and pushes the vault (supersedes "the vault is not a git repository"); Step 0 § 5's Step 3 exemption narrows to "when the last cloud run recorded a Step 3 failure". The vault's own `AGENTS.md` is unchanged — git is transport, not schema | assistant | open |
| 14g | Obsidian on the laptop: install the community plugin **Git**, set auto-pull every 10 min, auto commit-and-sync every 30 min with "pull before push", `pull.rebase` on; `git config user.name/email` in the vault | **Eriks**, with the assistant | open |

**Known costs, to accept or not at 14a:** the vault leaves the laptop; git
metadata inside iCloud Drive can be corrupted by iCloud's own sync if a
second Apple device also opens the vault with the plugin (mitigation: the
plugin on one device only, or move the vault out of iCloud); a same-minute
edit to `index.md` or `log.md` on both sides is a merge conflict — the cloud
run rebases once and otherwise stops with the conflict named, and the laptop
resolves it in Obsidian; `raw/` snapshots of mail written by the cloud run
carry mail text into the repository, which is the same class of content the
instance's ledgers already carry.

**What does not change:** the vault's schema (`../My Brain/AGENTS.md`), the
ingest workflow in `procedures/step-3-ingest.md`, the rule that only that
workflow writes there, and the security boundary — a clone is a read surface
plus the same single write surface.

## Known costs, accepted by choosing this option

- The instance (state, logs, briefs, ledgers) is copied to GitHub. The
  ledgers contain newsletter unsubscribe links with signed tokens; those are
  mailing-list tokens, not credentials, and the repository is private.
- Cron is UTC and does not follow Riga's DST.
- The routine bills against the Claude subscription's usage, on Opus.
- A wrong disposition in an unattended run is caught only when Eriks reads
  the brief — the same property Phase 1 already has, without a live chat to
  catch it sooner.
