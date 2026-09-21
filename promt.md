-- Kisi ek non-Jan-start customer ke purane aur naye dono era ke rows nikaalo, taaki pattern-shift ka time pata chale
SELECT strCustomerName, strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart
FROM tblMain
WHERE strCustomerName = 'IMPERIAL TRADING CO LLC'  -- start=6, humara already-verified clean customer
ORDER BY strMonthKey;

SELECT strCustomerName, strMonthKey, intFiscalYear, datFiscalYearStart
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
ORDER BY strMonthKey;

Stop on hunk (f). New finding: datFiscalYearStart is NOT a single consistent
"old convention" across the database — for every non-January fiscal-start-
month, roughly 30-40% of rows show YEAR(datFiscalYearStart) = intFiscalYear,
and 60-70% show YEAR(datFiscalYearStart) = intFiscalYear - 1, at scale
(hundreds of rows each way, not noise). Additionally, 28 customers have
MULTIPLE distinct datFiscalYearStart values within a single customer+
intFiscalYear — which is invalid regardless of which convention is
"correct," since this should be constant per fiscal year.

Do not assume "preserve old behavior" is well-defined for this column. Before
touching hunk (f) again: which of the two coexisting patterns matches legacy
MS Access's actual datFiscalYearStart values for the same customer/months?
I'll provide a legacy Access comparison for a specific customer across both
eras. Hold this hunk out of the diff until that's resolved — proceed with
everything else (a-e, g-j) as already reviewed.
