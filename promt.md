SELECT COUNT(*) AS WronglyUpdatedRecords
FROM tblMain
WHERE intFiscalYear = 2025
  AND datFiscalYearStart = '2026-01-01';
