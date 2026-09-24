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
SELECT s.strCustomerName, s.strMonthKey, s.intFiscalYear, s.intFiscalMonth,
       s.StoredYTD, s.SumMonthlyPBT
FROM (
  SELECT z.strCustomerName, z.strMonthKey, z.intFiscalYear, z.intFiscalMonth,
         z.curProfitBeforeTaxesYTD AS StoredYTD,
         (SELECT Sum(m.curProfitBeforeTaxes)
            FROM tblMain AS m
           WHERE m.strCustomerName = z.strCustomerName
             AND m.intFiscalYear = z.intFiscalYear
             AND m.intFiscalMonth <= z.intFiscalMonth) AS SumMonthlyPBT
  FROM tblMain AS z
  WHERE z.curProfitBeforeTaxesYTD Is Not Null
    AND z.curProfitBeforeTaxesYTD <> 0
) AS s
WHERE s.SumMonthlyPBT Is Not Null
  AND Abs(s.StoredYTD - s.SumMonthlyPBT) > 1
ORDER BY s.strCustomerName, s.strMonthKey;



SELECT z.strCustomerName, z.strMonthKey, z.intFiscalYear, z.intFiscalMonth,
       z.curProfitBeforeTaxesYTD AS StoredYTD,
       Sum(m.curProfitBeforeTaxes) AS SumMonthlyPBT
FROM tblMain AS z INNER JOIN tblMain AS m
  ON (z.strCustomerName = m.strCustomerName)
 AND (z.intFiscalYear = m.intFiscalYear)
WHERE z.curProfitBeforeTaxesYTD Is Not Null
  AND z.curProfitBeforeTaxesYTD <> 0
  AND m.intFiscalMonth <= z.intFiscalMonth
GROUP BY z.strCustomerName, z.strMonthKey, z.intFiscalYear,
         z.intFiscalMonth, z.curProfitBeforeTaxesYTD
HAVING Sum(m.curProfitBeforeTaxes) Is Not Null
   AND ((z.curProfitBeforeTaxesYTD - Sum(m.curProfitBeforeTaxes)) > 1
     OR (z.curProfitBeforeTaxesYTD - Sum(m.curProfitBeforeTaxes)) < -1)
ORDER BY z.strCustomerName, z.strMonthKey;
