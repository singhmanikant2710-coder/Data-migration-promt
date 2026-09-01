-- Pehle dekho (SELECT):
SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey = '202606';

-- Phir theek karo (UPDATE) — October start ke hisaab se 2025/9:
UPDATE tblMain
SET intFiscalYear = 2025,
    intFiscalMonth = 9
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey = '202606';

  SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
ORDER BY strMonthKey DESC;
