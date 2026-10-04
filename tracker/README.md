# Tracker — Google Sheet

This fork appends to an **existing** Google Sheet tab rather than creating a local `.xlsx`.

Requirements:
- The sheet is reachable through the Google Sheets connector in the account the scheduled task runs as.
- Row 1 is a header row. List its columns, in order, in `COLUMNS` in your config. The skill writes each new row in that order.
- At minimum, have columns for: link, area, rent, furnishing, available from, bedrooms, status, next action.

The skill:
- reads all existing rows first, to dedup by listing ID (from any URL) and by street + outward postcode
- appends new rows only. It never edits, reorders or deletes existing rows.
- sets Status to `NEW_STATUS` (e.g. `New (hunt)`), and starts Next action with `[Hunt <date> · <PRIORITY>]`, so you can filter hunt-found rows
- reads the appended range back to confirm the row count
