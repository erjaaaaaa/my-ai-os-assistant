# Sources — how to activate one later

Each file here is one source adapter: its `configured:` status, its control
query, its watermark key, its tools, and what it may and may never write.
Step 0 reads every file in this folder and runs every control query.

**Three findings that must never be confused:** `not configured` (never set
up — reported, not raised as a defect), `broken` (authorised but the control
query did not populate — an outage, reported as one), and `genuinely empty`
(control populated, sweep found nothing — a real result).

## Activating a source

1. Authorise the connector in the desktop app (Settings → Connectors), or
   complete the script's one-time setup.
2. Run the source's control query from its adapter file. **Do not proceed on an
   empty result** — that is the `broken` case, not the `live` one.
3. Set `configured: true` in the adapter file's frontmatter.
4. **Decide the watermark deliberately.** Zero means the next run sweeps the
   entire history and may create a hundred tasks. Setting it to now means
   anything older is never seen. Both consequences are real; a sensible
   default is the last few days. Write the chosen value into
   `state/state.json` under the source's key.
5. Run the routine once and read the Personal brief before trusting it.

## Adding a source that has no adapter here

Write a new `<id>.md` with the same frontmatter shape, a control query that is
independent of any time window, a watermark key and unit, an explicit tools
list matched on stable tool names, an allowed-writes line, and a forbidden
line. Then add its classification hints to `config/routing-rules.md` and its
row to the Sources list in `AGENTS.md`. A source without a control query
cannot be told apart from a broken one and must not be added.
