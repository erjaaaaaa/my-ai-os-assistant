# Ledgers

Three CSV files, written only by `procedures/step-1-inbox.md` and read by
`procedures/digest.md`:

- `receipts.csv` — charge_date, vendor, amount, currency, invoice_no, subject,
  messageId, threadId, threadUrl, source, notes
- `newsletters.csv` — received_date, from_name, from_address, source_domain,
  subject, threadId, messageId, threadUrl, list_id, list_unsubscribe, digested
- `promotions.csv` — same columns as newsletters

**Migrated on 2026-09-07** (see `logs/run-log.md`). Before that they did not exist until the historical data had been migrated from the
Google Sheets that Eriks's earlier automation wrote (Eriks, 2026-09-07:
"Before writing into CSVs … I'd like to migrate the data from the existing
google sheets so we don't lose the historical data"). The Drive connector is
signed in as a different Google account and cannot see those sheets, so the
migration runs from CSV exports: Eriks downloads each tab (File → Download →
Comma Separated Values) into `ledgers/import/` as `receipts.csv`,
`newsletters.csv`, `promotions.csv`; the assistant then loads them, maps the
sheet's `sent` column to `digested` (`TRUE` → `migrated-sent`, empty → empty),
dedupes on `messageId`, writes the three ledgers, verifies the row counts
against the exports, and commits.

**If a ledger file is ever missing, the inbox sweep labels the ledger classes
but writes no row and archives nothing in them** — it reports "ledger not
migrated" instead. A missing ledger is never created empty by a sweep.
