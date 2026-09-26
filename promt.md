SELECT strMonthKey, dblFixedChargeCoverage, dblFixedChargeCoverageTTM,
  curCashAvailableForFixedChargesTTM, curFixedChargesTTM
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202601' AND '202603'
ORDER BY strMonthKey;


SELECT strMonthKey, dblFixedChargeCoverage, dblFixedChargeCoverageTTM,
  curCashAvailableForFixedChargesTTM, curFixedChargesTTM
FROM tblMain
WHERE strCustomerName Like "ATHENS PAPER*"
  AND strMonthKey Between "202601" And "202603"
ORDER BY strMonthKey;
