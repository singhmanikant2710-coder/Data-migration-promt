READ-ONLY. Find how "Add New Month" creates a new tblMain row and sets intFiscalYear/intFiscalMonth. Quote with paths.

FIX APPROACH (decided): Fiscal conventions differ per customer (January-start: fiscalYear=calendarYear; April/July/October-start: varies). So instead of a formula, the new month takes the PREVIOUS (latest existing) month's fiscal values and increments:
  new fiscalMonth = (prev.fiscalMonth == 12) ? 1 : prev.fiscalMonth + 1
  new fiscalYear  = (prev.fiscalMonth == 12) ? prev.fiscalYear + 1 : prev.fiscalYear
This is convention-independent and matches existing data for all customers.

1) Find the backend add-month / create-row logic (search: add month, PUT /api/v1/main/row, UpsertRow, or where a NEW tblMain row's intFiscalYear/intFiscalMonth are assigned). Quote it.
2) How are intFiscalYear/intFiscalMonth currently computed for the new month? (Calendar-based, we believe — that's the bug.) Quote.
3) In that code path, can we query the PREVIOUS month's row (the latest existing month before the new one, for the same customer) to read its intFiscalYear/intFiscalMonth? Quote how existing months/rows are accessed there, or how we'd fetch MAX(strMonthKey) < newMonthKey.
4) Exact location to apply the previous-month + 1 logic.

OUTPUT:
- A) Current intFiscalYear/intFiscalMonth assignment on add-month, quoted.
- B) How to fetch the previous month's fiscal values in that code, quoted.
- C) Exact fix location.
- No fix. Findings only.
