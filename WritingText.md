SELECT m.strCustomerName, m.strMonthKey, m.intFiscalYear,
  CASE WHEN CAST(RIGHT(m.strMonthKey,2) AS int) >= c.intFiscalYearMonthStart
       THEN CAST(LEFT(m.strMonthKey,4) AS int) + 1
       ELSE CAST(LEFT(m.strMonthKey,4) AS int) END AS ExpectedFY
FROM tblMain m
JOIN tblCustomer c ON c.strCustomerName = m.strCustomerName
WHERE m.strCustomerName IN ('KEYSTONE PRIVATE INCOME FUND','ATHENS PAPER COMPANY INC')
  AND m.strMonthKey IN ('202104','202110','202510','202604')
ORDER BY m.strCustomerName, m.strMonthKey;

SELECT m.strCustomerName, COUNT(*) AS TotalMonths,
  SUM(CASE WHEN m.intFiscalYear <>
        CASE WHEN CAST(RIGHT(m.strMonthKey,2) AS int) >= c.intFiscalYearMonthStart
             THEN CAST(LEFT(m.strMonthKey,4) AS int) + 1
             ELSE CAST(LEFT(m.strMonthKey,4) AS int) END
      THEN 1 ELSE 0 END) AS WrongFY
FROM tblMain m
JOIN tblCustomer c ON c.strCustomerName = m.strCustomerName
WHERE m.strCustomerName IN ('KEYSTONE PRIVATE INCOME FUND','ATHENS PAPER COMPANY INC')
GROUP BY m.strCustomerName;
