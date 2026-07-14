UAT #82, #83, #84, #85 — Review History screen. Four related changes, all in the same screen. Apply them together.

I'm attaching the client's screenshots showing the Review History screen with each item marked.

CHANGE 1 (#82) — Remove the "Sample Name" dropdown.
Client: "We can remove this drop-down. This screen is meant to show all Finalized reviews ([Review_finalized_date] Is Not Null) from all Closed Samples."
- Remove the Sample Name select control entirely.
- The screen must now always show ALL finalized reviews across all closed samples — i.e. the underlying query must filter on [Review_finalized_date] IS NOT NULL and no longer filter by a selected sample.
- Report which backend query/endpoint feeds this screen and update it accordingly.

CHANGE 2 (#83) — Remove the "Borrower Name" search box and its Search button.
Client: "Remove this Borrower Name specific search in favor of the 'Google' search filter we have on Review Queue and Review Status."
- Remove the Borrower Name input + Search button.
- ADD the same global "Filter rows" search box that Review Queue and Review Status already have (a client-side text filter across the grid's columns). REUSE that existing pattern — do not build a new one.

CHANGE 3 (#84) — Rename and rebind the "COMPLETED" column.
Client: "Edit this date column to show Review_finalized_date value instead and label the column 'Finalized'."
- Column header: change "COMPLETED" to "FINALIZED".
- The value must come from [Review_finalized_date] (not whatever it currently shows). Update the backend query/DTO if that field is not already returned.

CHANGE 4 (#85) — Remove the Refresh and Close buttons.
Client: "Remove the Refresh and Close buttons. They serve no purpose in the new application environment."
- Remove both buttons from the Review History header.

FIRST report (read-only): show me the Review History page component, the backend query/endpoint that feeds it, and confirm whether [Review_finalized_date] is already in the payload. Then apply all four changes.

Constraints:
- Reuse the existing global filter pattern from Review Queue for change 2.
- Do not break the existing grid, sorting, or pagination.
- Use the LIVE DB, ignore columns.csv.

Report findings, then apply. Show me the diffs.
