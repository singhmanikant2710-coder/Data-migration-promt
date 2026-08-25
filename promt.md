READ-ONLY. Read these two files ONCE each, then STOP. Do not read other files.

1) legacy/exported_legacy/queries.sql
2) legacy/exported_legacy/VBA_Form_frm005IndirectAutoMain.txt

I already confirmed (in queries.sql) that curInterestExpenseTTM = SUM of monthly curInterestExpense over 12 months (tblLookupMonthkey.ID between cboMonthLoadID-11 and cboMonthLoadID). Now I need the DISPLAY ratio formulas for IndirectAuto. Read the FULL file before answering.

Find and quote VERBATIM (filename + surrounding lines):

1) Interest Coverage TTM display formula — search the IndirectAuto Main form for expressions bound to "InterestCoverage", "IntCoverageTTM", "perInterestCoverageTTM", "Coverage". Quote the exact numerator/denominator. Is it curEBITTTM / curInterestExpenseTTM, or CashAvailableForFixedChargesTTM / InterestExpenseTTM, or another pair?

2) EBIT TTM composition — search "EBIT", "EBITTTM". Quote how it's built (e.g. curProfitBeforeTaxesTTM + curInterestExpenseTTM).

3) In queries.sql, search for any UPDATE/SELECT computing "perInterestCoverageTTM" or a coverage ratio dividing by curInterestExpenseTTM. Quote it.

For each: filename + verbatim quote + one plain line. If genuinely absent after reading the FULL file, say "NOT FOUND after full read". Then STOP.
