SELECT strCustomerName, intFiscalYear, COUNT(DISTINCT datFiscalYearStart) AS DistinctStarts
FROM tblMain
WHERE datFiscalYearStart IS NOT NULL
GROUP BY strCustomerName, intFiscalYear
HAVING COUNT(DISTINCT datFiscalYearStart) > 1
ORDER BY strCustomerName, intFiscalYear;

Before I approve: two direct questions, need unambiguous answers, not prose.

1. Does this diff's DeriveFiscalYearStartDate REPLACE the computation used
   in yesterday's approved hunk (f) — new DateTime(fy, fiscalStartMonth, 1)
   — inside UpsertRowWithConnectionAsync? Yes or no.

2. If yes: after this diff is applied, if Athens Paper's 202510 row is
   saved again, what value will datFiscalYearStart get — 2025-10-01 or
   2026-10-01? This is a direct behavior question about the same test
   case we already verified against legacy yesterday (which returned
   2026-10-01). I need to know if this diff changes that going forward.

Do not apply anything until both are answered explicitly.
