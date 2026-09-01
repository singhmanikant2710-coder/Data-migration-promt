SELECT 
    COUNT(*) AS TotalRecords,
    MIN(datFiscalYearStart) AS MinFiscalYearStart,
    MAX(datFiscalYearStart) AS MaxFiscalYearStart,
    MIN(datFirstOfMonth) AS MinFirstOfMonth,
    MAX(datFirstOfMonth) AS MaxFirstOfMonth
FROM tblMain
WHERE intFiscalYear = 2025;


SELECT TOP 20
    strCustomerName,
    strMonthKey,
    datFiscalYearStart,
    datFirstOfMonth,
    intFiscalYearMonthStart,
    strFiscalYearMonthStart,
    intFiscalMonth,
    intFiscalYear
FROM tblMain
WHERE intFiscalYear = 2025
ORDER BY datFirstOfMonth DESC;
