SELECT strMonthKey, curCPLTD, curCPLTDTTM, curInterestExpenseTTM,
       curFixedChargesTTM, dblFixedChargeCoverageTTM
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey IN ('202602','202603');
