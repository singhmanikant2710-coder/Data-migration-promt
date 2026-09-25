SELECT strMonthKey, curProfitBeforeTaxesTTM FROM (
  SELECT LTRIM(RTRIM(strMonthKey)) AS strMonthKey,
    SUM(curProfitBeforeTaxes) OVER (PARTITION BY LTRIM(RTRIM(strCustomerName))
      ORDER BY LTRIM(RTRIM(strMonthKey))
      ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curProfitBeforeTaxesTTM
  FROM tblMain
  WHERE LTRIM(RTRIM(strCustomerName)) = 'ATHENS PAPER'
) x
WHERE strMonthKey = '202510';
