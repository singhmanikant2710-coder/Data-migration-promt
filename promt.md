SELECT TRY_CONVERT(int, intFiscalYearMonthStart) AS StartMonth,
       COUNT(*)                                  AS Customers,
       MIN(LTRIM(RTRIM(strCustomerName)))        AS ExampleCustomer
FROM dbo.tblCustomer
GROUP BY TRY_CONVERT(int, intFiscalYearMonthStart)
ORDER BY StartMonth;
