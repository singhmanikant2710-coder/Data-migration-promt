Hi John, double-checked Keystone against Athens Paper — both have an
October fiscal start, so their fiscal year labels should follow the same
rule (fiscal year named for the year it ends).

Result from Access tblMain:
- ATHENS PAPER: 116 months, 0 with an unexpected fiscal year
  (202104 = FY2021, 202110 = FY2022, 202510/202604 = FY2026)
- KEYSTONE: 62 months, all 62 one year higher
  (202104 = FY2022, 202110 = FY2023, 202510/202604 = FY2027)

Jay confirmed Oct 2025 – Sep 2026 should be FY2026 for Keystone, so it
needs the same relabel as Bankers, Nationwide and Vermeer.

Query used (Access):
SELECT m.strCustomerName, m.strMonthKey, m.intFiscalYear,
  IIf(Val(Right(m.strMonthKey,2)) >= c.intFiscalYearMonthStart,
      Val(Left(m.strMonthKey,4)) + 1, Val(Left(m.strMonthKey,4))) AS ExpectedFY
FROM tblMain AS m INNER JOIN tblCustomer AS c
  ON m.strCustomerName = c.strCustomerName
WHERE m.strCustomerName In ("KEYSTONE PRIVATE INCOME FUND","ATHENS PAPER COMPANY INC")
ORDER BY m.strCustomerName, m.strMonthKey;
