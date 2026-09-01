SELECT 
    datFiscalYearStart,
    COUNT(*) AS RecordCount
FROM tblMain
WHERE intFiscalYear = 2025
GROUP BY datFiscalYearStart
ORDER BY datFiscalYearStart;
