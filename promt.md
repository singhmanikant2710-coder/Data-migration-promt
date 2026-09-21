SELECT strMonthKey, strCovenantName, strCovenantThreshold, strCovenantActual, strCovenantReported, strCovenantDescription
FROM tblMainCovenants
WHERE strCustomerNumber = '84942562'
  AND strCovenantName LIKE '%Tangible%'
ORDER BY strMonthKey;

SELECT strMonthKey, strCovenantName, strThreshold1, strActual1, strReported1, strCovenantDescription1
FROM tblMainDisplayCovenants
WHERE strCustomerNumber = '84942562'
  AND strMonthKey = '202510';

  SELECT *
FROM v_ifu_covenants
WHERE CustomerId = '84942562'
ORDER BY MonthKey;
