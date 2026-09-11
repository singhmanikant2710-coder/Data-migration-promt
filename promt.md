SELECT
    strCustomerName,
    strMonthKey,
    intFiscalYear,
    intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'ADIR INTERNATIONAL LLC'
  AND strMonthKey = '202402';
