The files ARE in this repo. Exact relative paths from repo root (c:/Users/CC438/source/repos/fhn-bcat):

1) legacy/_exported/queries.sql
2) legacy/_exported/VBA_Form_frm005IndirectAutoMainCurrentEdit.txt

Note: the folder name starts with an underscore ("_exported"). It is not hidden — it exists. If your read tool skips underscore-prefixed folders, read the files by their full absolute paths instead:

1) C:\Users\CC438\source\repos\fhn-bcat\legacy\_exported\queries.sql
2) C:\Users\CC438\source\repos\fhn-bcat\legacy\_exported\VBA_Form_frm005IndirectAutoMainCurrentEdit.txt

READ-ONLY. Read these two files ONCE each, then STOP. Do not read any other file. Do not open the current app.

Find and quote VERBATIM (with filename + surrounding lines):
1) Interest Expense TTM formula — search "InterestExpenseTTM", "curInterestExpenseTTM", "InterestTTM". Is it a SUM of monthly interest over the trailing 12 months? How is the 12-month window bounded?
2) Interest Coverage TTM — search "InterestCoverageTTM", "perInterestCoverageTTM". Quote numerator/denominator.
3) EBIT TTM — search "EBITTTM", "curEBITTTM", "EBIT". Quote composition (PBT TTM + Interest Expense TTM, or other?).
4) The 12-month window bound (WHERE/date-range/TOP/SUM) — verbatim.
5) Any FiscalYear/FiscalMonth reference in the TTM calc.

For each: filename + verbatim quote + one plain line. If not found in these two files, say "NOT FOUND in these two files" (do not guess, do not open more files). Then STOP.
