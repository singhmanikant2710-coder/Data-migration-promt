READ-ONLY. Read these files ONCE (absolute paths, folder starts with underscore, it exists):
1) C:\Users\CC438\source\repos\fhn-bcat\legacy\_exported\VBA_Form_frm005IndirectAutoMain.txt
2) C:\Users\CC438\source\repos\fhn-bcat\legacy\_exported\queries.sql

Do NOT read other files. Find the Interest Coverage TTM and EBIT TTM display formulas, quote verbatim, then STOP.

1) Interest Coverage TTM: search "InterestCoverage", "IntCoverage", "Coverage" in the IndirectAuto form. Quote the exact expression that computes the displayed ratio. I need the exact numerator and denominator (e.g. curEBITTTM / curInterestExpenseTTM, or curCashAvailableForFixedChargesTTM / curInterestExpenseTTM).

2) EBIT TTM: search "EBIT", "EBITTTM". Quote how EBIT TTM is composed (e.g. curProfitBeforeTaxesTTM + curInterestExpenseTTM). 

3) In queries.sql, search for any query that computes "perInterestCoverageTTM" or a coverage ratio using curInterestExpenseTTM. Quote it if present.

For each: filename + verbatim quote + one plain line. If not found, say "NOT FOUND". Then STOP.
