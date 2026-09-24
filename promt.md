SELECT z.strCustomerName, z.strMonthKey, z.intFiscalYear, z.intFiscalMonth,
       z.curProfitBeforeTaxesYTD,
       (SELECT SUM(m.curProfitBeforeTaxes) FROM tblMain m
         WHERE m.strCustomerName = z.strCustomerName
           AND m.intFiscalYear = z.intFiscalYear
           AND m.intFiscalMonth <= z.intFiscalMonth) AS SumMonthlyPBT,
       (SELECT COUNT(*) FROM tblMain m
         WHERE m.strCustomerName = z.strCustomerName
           AND m.intFiscalYear = z.intFiscalYear
           AND m.intFiscalMonth <= z.intFiscalMonth
           AND m.curProfitBeforeTaxes IS NOT NULL) AS MonthsWithPBT
FROM tblMain z
WHERE z.curProfitBeforeTaxesYTD = 0
ORDER BY z.strCustomerName, z.strMonthKey;


Read-only, do not apply.
1. manufacturing.ts: show the FULL function block lines ~200-235 BEFORE
   and AFTER Hunk B, complete, with all braces. Include the
   "computed YTD sum" branch. Fix the hunk header counts.
   Simplify to `if (yNum === null)`; drop hasNonZeroYtd if unused.
2. Grep ALL files in frontend/src/blackbook/mappings/*.ts (and any
   other industry mapping) for the same pattern: YTD checked with
   Math.abs(...) > 0 or "missing/zero", then replaced by TTM. List
   file:line verbatim for each hit.
3. BlackbookSummaryService: list all 7 ytdPbt field definitions with
   line numbers (you listed 6 + "one more"). Confirm none uses PBT YTD
   as a DENOMINATOR or hides/skips on zero.
4. Also grep for any other YTD metric (revenue, sales, etc.) that is
   backfilled from TTM on zero. Report only; no fix.
