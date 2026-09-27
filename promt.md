Blackbook PDF (Fiscal YTD and Rolling 24): the "FCC TTM" column shows
the value with an "x" suffix (e.g. 4.70x); legacy PDF shows 4.70 (no x).
The value is correct — only the suffix differs.

Apply the same no-suffix rule already used in MonthSummaryTable /
DetailGrid (formatRatioNoSuffix, banker's 2 dp via formatRatio) to the
same labels in BlackBookPdf.tsx. Quote which labels that UI rule covers
and apply exactly those — do not change other ratio columns.

Also verify the VALUE source: for every month row, the PDF "FCC TTM"
column must read the stored dblFixedChargeCoverageTTM of THAT row — the
same field the Summary Top Strip FCC TTM uses. Quote the alias/pick
chain for that column. If any alias can resolve to perInterestCoverage,
dblFixedChargeCoverage (monthly) or a frontend recompute, remove it.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is

- SELECT strMonthKey, dblFixedChargeCoverageTTM
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202410' AND '202603'
ORDER BY strMonthKey;
