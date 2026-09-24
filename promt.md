SELECT z.strCustomerName, z.strMonthKey, z.intFiscalYear, z.intFiscalMonth,
       z.curProfitBeforeTaxesYTD AS StoredYTD,
       (SELECT Sum(m.curProfitBeforeTaxes)
          FROM tblMain AS m
         WHERE m.strCustomerName = z.strCustomerName
           AND m.intFiscalYear = z.intFiscalYear
           AND m.intFiscalMonth <= z.intFiscalMonth) AS SumMonthlyPBT
FROM tblMain AS z
WHERE z.curProfitBeforeTaxesYTD Is Not Null
  AND z.curProfitBeforeTaxesYTD <> 0;

  SELECT s.strCustomerName, s.strMonthKey, s.intFiscalYear, s.intFiscalMonth,
       s.StoredYTD, s.SumMonthlyPBT
FROM qryYtdCheck_Step1 AS s
WHERE s.SumMonthlyPBT Is Not Null
  AND Abs(s.StoredYTD - s.SumMonthlyPBT) > 1
ORDER BY s.strCustomerName, s.strMonthKey;
