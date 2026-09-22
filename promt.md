SELECT COUNT(*) FROM dbo.tblMain WHERE datFiscalYearStart IS NULL;

SELECT strCustomerName, COUNT(*) AS NullRows
FROM dbo.tblMain
WHERE datFiscalYearStart IS NULL
GROUP BY strCustomerName
ORDER BY NullRows DESC;
