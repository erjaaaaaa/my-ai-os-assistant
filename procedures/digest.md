# Weekly digest — newsletters and promotions

Runs on `/digest` only. NARROWED 2026-09-07 by Eriks: *"Digest should be
just sent once a week or on demand."* Superseded text: "or from Step 1 on
the first run on or after Friday 16:00 Europe/Riga each week". Reads the two
ledgers; writes one HTML deck and marks rows digested. Nothing is sent
anywhere. Card links use
`https://mail.google.com/mail/?authuser=epetersons87@gmail.com#all/<threadId>`
(the account-naming form; see `step-1-inbox.md` § 3 for why).

1. Read `ledgers/newsletters.csv` and `ledgers/promotions.csv`. Select rows
   whose `digested` is empty. Zero rows → say so in chat and stop; no file, no
   state change. If a ledger is missing, report "ledger not migrated" and stop.
2. For each row, `get_thread` (`PLAIN_TEXT`) by threadId. Summarise into: brand
   (the real sender — "FutureTools", not the delivery platform; fall back to
   the from-name), title (usually the subject), date, three to six concrete
   bullets (updates, launches, numbers, deadlines, policy actions — specific,
   no fluff), and a one-sentence elevator summary. A thread that cannot be read
   gets a card saying so; never a made-up summary.
3. Build the deck with the `frontend-slides` skill: a title slide ("Weekly
   Newsletter Digest — YYYY-MM-DD", count of newsletters and distinct brands),
   one card-slide per newsletter (brand • domain • date • open link; one-liner;
   bullets), then a divider and the promotions cards the same way. Save it as
   `briefs/digest-YYYY-MM-DD.html`.
4. Show it in chat with `SendUserFile` (render). Verify the file exists.
5. Set `digested` = today's date on every row that was included; re-read the
   ledger to confirm the count. Set `digest.last_run_date` in state. Append a
   line to the run log (rows digested per ledger, file path). Commit.
