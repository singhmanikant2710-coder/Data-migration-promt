SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
ORDER BY strMonthKey DESC;

SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey >= '202606'   -- jo test mein add kiye (202606 se aage)
ORDER BY strMonthKey DESC;

-- Pehle dekho kitne delete honge (SELECT):
SELECT COUNT(*) AS test_months
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey > '202605';

-- Confirm karke, delete (test months, 202605 tak original rakho):
DELETE FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey > '202605';

  SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
ORDER BY strMonthKey DESC;
