READ-ONLY. Read ONLY these two files, ONCE each, then STOP:
1. legacy\_exported\queries.sql
2. legacy\_exported\VBA_Form_frm005IndirectAutoMainCurrentEdit.txt

Do NOT read any other file. Do NOT read the current app. Find the legacy TTM interest formulas, quote verbatim, stop. No loop.

In those two files, find and quote VERBATIM (with which file + surrounding lines):

1) Interest Expense TTM: search for "InterestExpenseTTM", "curInterestExpenseTTM", "InterestTTM", "Interest Expense TTM". Quote the exact SQL or VBA that computes it. Is it a SUM of monthly interest over the trailing 12 months? How is the 12-month window bounded (row count / date range / fiscal filter)?

2) Interest Coverage TTM: search "InterestCoverageTTM", "Interest Coverage TTM", "perInterestCoverageTTM". Quote the exact numerator/denominator formula.

3) EBIT TTM: search "EBITTTM", "curEBITTTM", "EBIT". Quote its composition (is it PBT TTM + Interest Expense TTM, or other?).

4) The 12-month window: quote the exact WHERE/date-range/TOP/SUM that bounds the trailing 12 months in the TTM computation.

5) Fiscal year: quote any FiscalYear/FiscalMonth reference in the TTM calc (to know if TTM resets at fiscal year-end).

OUTPUT:
- For each 1-5: filename + verbatim quote + one plain line of what it does.
- If not found in these two files, say "NOT FOUND in these two files" for that item (do not guess, do not open more files).
- Then STOP.
