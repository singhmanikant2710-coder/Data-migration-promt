BEGIN TRANSACTION;

UPDATE tblMain
SET intFiscalYear = 2026
WHERE intFiscalYear = 2025
  AND datFiscalYearStart >= '2026-01-01'
  AND datFiscalYearStart < '2027-01-01';

SELECT @@ROWCOUNT AS UpdatedRecords;
