DELETE FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey > '202605';

  SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
ORDER BY strMonthKey DESC;
