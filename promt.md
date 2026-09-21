SELECT strCustomerName, intFiscalYear, COUNT(DISTINCT datFiscalYearStart) AS DistinctStarts
FROM dbo.tblMain
WHERE datFiscalYearStart IS NOT NULL
GROUP BY strCustomerName, intFiscalYear
HAVING COUNT(DISTINCT datFiscalYearStart) > 1;

SELECT c.intFiscalYearMonthStart AS StartMonth,
       (YEAR(m.datFiscalYearStart) - m.intFiscalYear) AS YearDelta,
       COUNT(*) AS RowCount
FROM dbo.tblMain m
JOIN dbo.tblCustomer c ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName))
WHERE m.datFiscalYearStart IS NOT NULL AND m.intFiscalYear IS NOT NULL
GROUP BY c.intFiscalYearMonthStart, (YEAR(m.datFiscalYearStart) - m.intFiscalYear)
ORDER BY StartMonth, YearDelta;

SELECT DISTINCT strMonthKey FROM tblMain WHERE strMonthKey NOT LIKE '[0-9][0-9][0-9][0-9][0-9][0-9]';
