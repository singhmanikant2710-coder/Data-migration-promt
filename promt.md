READ-ONLY. Read frontend/src/app/blackbook/edit/page.tsx ONCE only. Do not re-read, do not open other files. Answer two narrow questions, then stop.

1) Find the function normalizeEditsForBackend. Quote its full body verbatim. (I need to see how a user's entered month values — e.g. month PBT, interest expense — are keyed before the PUT to /api/v1/main/batch. Specifically whether they map to keys like curInterestExpense, curFixedCharges, curEBITDA, curCashAvailableForFixedCharges.)

2) Find every line containing the string "edits[" AND every line that reads or spreads `edits` into the data passed to buildMonthSummaryColumns or into series/enrichedSeries. Quote those lines only (with the 2 lines above and below each for context).

Output only: the normalizeEditsForBackend body, then the edits[...] lines. Then stop. Do not analyze, do not propose anything.
