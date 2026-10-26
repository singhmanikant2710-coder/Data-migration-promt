SELECT m.strCustomerName, m.strMonthKey, m.intFiscalYear,
  IIf(Val(Right(m.strMonthKey,2)) >= c.intFiscalYearMonthStart,
      Val(Left(m.strMonthKey,4)) + 1, Val(Left(m.strMonthKey,4))) AS ExpectedFY
FROM tblMain AS m INNER JOIN tblCustomer AS c ON m.strCustomerName = c.strCustomerName
WHERE m.strCustomerName In ("KEYSTONE PRIVATE INCOME FUND","ATHENS PAPER COMPANY INC")
  AND m.strMonthKey In ("202104","202110","202510","202604")
ORDER BY m.strCustomerName, m.strMonthKey;

SELECT m.strCustomerName, Count(*) AS TotalMonths,
  Sum(IIf(m.intFiscalYear <> IIf(Val(Right(m.strMonthKey,2)) >= c.intFiscalYearMonthStart,
      Val(Left(m.strMonthKey,4)) + 1, Val(Left(m.strMonthKey,4))), 1, 0)) AS WrongFY
FROM tblMain AS m INNER JOIN tblCustomer AS c ON m.strCustomerName = c.strCustomerName
WHERE m.strCustomerName In ("KEYSTONE PRIVATE INCOME FUND","ATHENS PAPER COMPANY INC")
GROUP BY m.strCustomerName;
