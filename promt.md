SELECT
    strCustomerName,
    strMonthKey,
    intFiscalYear,
    intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'ADIR INTERNATIONAL LLC'
  AND strMonthKey = '202412';

  UPDATE tblMain
SET
    intFiscalYear = 2025,
    intFiscalMonth = 11
WHERE LTRIM(RTRIM(strCustomerName)) = 'ADIR INTERNATIONAL LLC'
  AND strMonthKey = '202412';

  SELECT
    strCustomerName,
    strMonthKey,
    intFiscalYear,
    intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'ADIR INTERNATIONAL LLC'
  AND strMonthKey IN ('202411', '202412', '202501')
ORDER BY strMonthKey;
