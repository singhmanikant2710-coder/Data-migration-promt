SELECT strCustomerName, strMonthKey, curProfitBeforeTaxes, curProfitBeforeTaxesYTD,
  perInterestCoverageTTM, per60DPD, perNetChargeOffTTM, curNetChargeOffTTM,
  curAveragePrincipalNRTTM, curAverageGrossNRTTM,
  strPrincipalOrGrossCalculationSelectionNetChargeOff, perReserveCoverage,
  strCustomField1, strCustomField2, strCustomField3, strCustomField4
FROM tblMain
WHERE strCustomerName IN ('MARINER FINANCE LLC','AMERICAN CREDIT ACCEPTANCE','SHABANA MOTORS LLC')
  AND strMonthKey = '<MONTH>';

SELECT strMonthKey, strCovenantName, intCovenantOrder, strCovenantFormat, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName = 'MARINER FINANCE LLC' AND strMonthKey = '<MONTH>'
ORDER BY intCovenantOrder;
