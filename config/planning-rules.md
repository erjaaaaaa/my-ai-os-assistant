# Planning rules — sizing and capacity

Read by `PLANNING.md` at the start of every planning run, and by Step 1 for
the auto-sizing and full-label-set rules. Policy lives here; steps live in
`PLANNING.md`.

## Sizing

Every schedulable task carries exactly one `size/*` label. **The label is the
single source of truth for size**; any duration field Todoist shows in its own
UI is a non-authoritative mirror of it.

```
size/XS  ≤ 5m  →   5m      size/L    1–4h    → 180m
size/S   ≤ 15m →  15m      size/XL   ~8h     → 480m
size/M   ≤ 1h  →  45m      size/XXL  > 1 day → not schedulable
```

- **Absence of a `size/*` label means unsized. There is no `size/?`.** Never
  write a placeholder for "unknown" — the empty state is the empty state.
- **`size/XXL` is a routing signal, not a size.** It never enters a plan; it is
  listed under "Not planned, and why" as *needs splitting*, and a split happens
  only on Eriks's explicit per-item yes.

**Heuristic.** The title's prefix carries most of the signal (`Decide:`,
`Delegate:`); otherwise the verb. Confirm / RSVP / forward / a short reply →
**XS**; call / book / order / anything needing a short composition → **S**;
research / compare / fill in a form / prepare documents → **M**; plan /
organise / build / set up (a trip, a purchase, a home project) → **L**. The
test: *could this be finished in one sitting, and would it need a protected
block?* Yes to the second means at least L.

**Auto-sizing.** Triage labels a task itself **only where the heuristic lands
XS, S or M** — routine items where a wrong guess costs minutes. **L and above
is never guessed: leave it unsized and ask.** L spans 1–4h on one flat value,
so guessing inside it invents or loses hours.

## Capacity — a declared budget, not a workday

Eriks has no fixed hours for this instance: it runs on spare time, picked up
when found. Eriks, 2026-09-07: "Assume 2–3 hours a day available. We'll
calibrate the plan during the planning sessions." So capacity is **not**
derived from a calendar window. It is a declared budget:

```
budget_per_day     = 150m          (tunable; the 2–3 h midpoint)
commit_fraction    = 0.8           (tunable; leaves room for the unplanned)
capacity_per_day   = budget_per_day × commit_fraction = 120m
capacity_per_week  = capacity_per_day × plan_days_per_week (default 7)
protected_block    = 90m           (tunable; the minimum block an L task needs)
```

Four tunable parameters: daily budget, commit fraction, plan days per week,
protected block. Change them here, after a planning session shows they are
wrong, and note the change in the plan file that prompted it.

The calendar is still read in every planning run — for **conflicts and
preparation**, never for capacity. If it is unreachable, the plan header says
`calendar: UNREACHABLE` and every prep-dependent item is marked as unverified;
capacity is unchanged because it never depended on the calendar.

**A capacity of 0 does not forbid placing a commitment someone is waiting
on.** The formula answers *how much new work should be committed to*, not *how
many minutes exist*. Place it and print
`planned 15m / capacity 0m — OVER-COMMITTED BY 15m`. **Never pro-rate or
inflate either number to make them agree.**

A day plan reserves one protected block for the largest item rather than
tiling six perfect small items nobody executes.

## The full-label-set rule — governs every run that writes a label, planning or not

A label update **REPLACES** the set; it does not append. Sending one label to
a task carrying three leaves it carrying one; the write returns success and
nothing warns you. So: **re-read the task's current labels immediately before
the write** (never from a subagent's report or an earlier snapshot); **send the
complete desired set** — current, plus additions, minus only deliberate
removals; **verify by diff** that no pre-existing label was lost; and **never
write a label from a task list you did not just read.** Eriks's own labels
(`book`, `health`, `call`, `life`, `plan`, `admin`, `home`, `buy`, …) travel
with the task through every write.

## Never scored

Tasks carrying `someday` or `agent-waiting` are never sized, never planned,
never challenged. Subtasks are sized and dated in place under their parent and
never moved to a section.
