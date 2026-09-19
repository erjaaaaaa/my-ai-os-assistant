---
name: digest
description: Build Eriks's weekly newsletter-and-promotions digest as an HTML slide deck from the personal instance's ledgers and show it in chat. Use when Eriks says "/digest", "newsletter digest", "what did the newsletters say this week", or "summarise my newsletters". Scoped to the personal instance folder.
---
INSTANCE_ROOT: the folder that holds `AGENTS.md` — the repository root. On
Eriks's laptop that is
`/Users/erik.peterson/Library/Mobile Documents/com~apple~CloudDocs/My AI OS/Assistant`;
in a cloud routine it is the working directory of the clone. (CHANGED
2026-09-19 from the fixed laptop path so the same skill runs unattended.)

Read `AGENTS.md` under INSTANCE_ROOT, then execute `procedures/digest.md`.

Constraints: nothing is sent to anyone; the deck is a file in `briefs/` shown
in chat; a ledger that does not exist means "not migrated", not "empty";
summaries come only from threads actually read, never invented; rows are
marked digested only after the file is verified on disk.
