READ-ONLY — do not edit any files.

Previous investigation guessed a column "dblCovenantThreshold{N}" on tblMain
for reading covenant threshold values — this column DOES NOT EXIST. Confirmed
via full INFORMATION_SCHEMA.COLUMNS dump: tblMain has only dblCovenantActual1..6,
their...Formatted variants, strCovenantName1..6, and a single non-numbered
strThreshold (varchar) — no numbered Threshold column at all.

Two other tables/views may be relevant:
- tblMainDisplayCovenants: columns strCustomerName, strMonthKey, strCovenantName
  (singular), strThreshold1..6, strActual1..6, strReported1..6.
- vw_BCAT_WholesaleTradeIndustryFinancialUpdate_Final_v1 (and sibling views per
  industry): columns Covenant1Threshold..Covenant4Threshold. Athens Paper's
  industry is WholesaleTrade.

In SqlMainRepository.cs, find MapMetricPoint (previously cited around lines
1524-1533) and quote the EXACT code VERBATIM — not paraphrased, not a guessed
column name — for how a threshold-type key like "MinTangibleNetWorth" or
"Min Tangible Net Worth" is resolved for the GET response. State precisely:
- Which table/view it queries
- The exact column or expression used
- Whether it falls back to an industry-template view when no customer-specific
  value exists, and under what condition

Also re-verify: does GET's "MinTangibleNetWorth" field for Athens Paper,
month 202510, actually come from tblMain.dblCovenantActual1 (confirmed 0/NULL
in our database), tblMainDisplayCovenants.strThreshold1 (or another slot),
or the WholesaleTrade industry view's Covenant{N}Threshold? Quote the code
that decides this, do not infer.

Report the verbatim code and your conclusion. Do not propose or write a fix.
