# Spine section — Maintenance and the learning loop

Moved out of the always-loaded `AGENTS.md` on 2026-11-01 by Eriks's decision (*"Yes, split it"*, answering "Split AGENTS.md into a short core plus on-demand files?"). The text below is **unchanged** — copied verbatim from the original line ranges named in each block. `AGENTS.md` keeps a stub with the rules that must hold even when this file is unread, and says when to read it.

---

<!-- original AGENTS.md lines 623–693 -->

## Maintenance

Kept current, and by whom:

- `state/state.json` — ids re-verified by name when `_verified` is older than
  30 days; watermarks advanced only at Step 5. The assistant.
- `procedures/cloud-run.md` and the cloud routine's registration (schedule,
  model, connectors) — Eriks, with the assistant. ADDED 2026-09-19.
- `config/tools.md` — fixed in the run that finds drift. The assistant.
- `config/sources/*.md` — `configured:` flipped only after a populated
  control query, per `config/sources/README.md`. Eriks, with the assistant.
- § Phase gates above — edited only by Eriks, in writing.
- `config/voice-style-guide.md` — refreshed when drafts start needing tone
  edits. The assistant, on Eriks's request.
- `config/routing-rules.md` § Source-specific notes and § Mail label taxonomy
  — the tunable scope and the label classes.
  Eriks, or the assistant applying an answered `[Needs Eriks]` task.

### The learning loop

**CHANGED 2026-11-01 by Eriks, in chat:** *"keep just the log of changes (which doesn't need to be read on each start)"* — `lessons-learned.md` is **no longer read at the start of a run.** Live rules are in the governing files and in `config/rules-digest.md`, which Step 0 reads in its place. Appending a correction to `lessons-learned.md` is unchanged. **Unattended cloud runs only append: they never edit `AGENTS.md`, config files or the digest.** A new durable rule reaches the digest through the promotion review on the laptop, with Eriks's yes. Whether agents may later edit `AGENTS.md` themselves is **not decided** (default: no). Superseded text below: "is read at the **start of every run**".

**`lessons-learned.md`** is read at the **start of every run** and every rule
in it is applied. When something goes wrong, or Eriks corrects the
assistant's behaviour, append one entry:

```
- YYYY-MM-DD — **Short title** — What happened. **Rule:** what to do differently.
```

**Keep rules concrete and testable.** "Be more careful" is not a rule; "never
create a task from a thread where Eriks is only CC'd" is. Never reword or
delete an existing entry — if a rule changes, append a new one saying which it
replaces.

**`lessons-learned-archive.md`** holds the full verbatim text of every
promoted entry, in chronological order. **It is not read at startup** — that
is the whole point of the split. Grep it when you need the *why* behind a rule
found in a governing file, or to audit whether a promotion was faithful.

**`config/methods.md`** holds one-line distilled *working-method* rules —
verification habits, defaults, when to ask — whose only home would otherwise
be an entry's general-rule paragraph. It **is** read at startup, so keep it
terse: one bullet, about two lines, citing its archived entry. Anything longer
belongs in a procedure file.

**The promotion review** runs periodically over every entry **older than 7
days**, and does exactly one of four things per entry:

- **verify-and-archive** — the rule is demonstrably already in a governing
  file, with the evidence named. "Probably covered" is not evidence.
- **promote-and-archive** — a clear, dated Eriks decision: consolidate it
  verbatim into the governing file, verify the edit by reading it back, then
  move.
- **distill-and-archive** — a method rule with no procedural home: one bullet
  in `config/methods.md`, then move.
- **ask the owner** — promotion needs interpretation. The entry stays until
  answered, as a `[Needs Eriks]` task.

**Entries younger than 7 days always stay**, as does anything whose promotion
needs interpretation. **An entry moves only when nothing in it would be lost
from the read path.** Moves are exact line ranges, verified verbatim after
writing; entries are **never reworded in either file.**

**Log every review run — including a zero-work one** — with counts and,
critically, the **NET read-path delta: the sum across every file the routine
opens at startup, not just this file's shrinkage.** Promotion **relocates and
compresses roughly 10:1 rather than removing**, and the file method rules are
distilled into is on the read path too. A review that reports only the hot
file's reduction is measuring itself with the one number guaranteed to
flatter it.

All paths are relative to this folder.

