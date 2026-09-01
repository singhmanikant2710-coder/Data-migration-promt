SELECT COUNT(*) AS AffectedRecords
FROM tblMain
WHERE intFiscalYear = 2025
  AND datFiscalYearStart >= '2026-01-01'
  AND datFiscalYearStart < '2027-01-01';
