SELECT COLUMN_NAME, DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMainCovenants'
  AND COLUMN_NAME IN ('dblActual', 'Actual', 'strCovenantActual');


  SELECT strCovenantName, dblActual, strCovenantActual, dblThreshold
FROM tblMainCovenants
WHERE strCustomerName = 'NATIONWIDE SPECIALTY FINANCE INC'
  AND strMonthKey = '202601';

  SELECT strCovenantName1, dblCovenantActual1, strCovenantName2, dblCovenantActual2,
       strCovenantName3, dblCovenantActual3, strCovenantName4, dblCovenantActual4,
       strCovenantName5, dblCovenantActual5, strCovenantName6, dblCovenantActual6
FROM tblMain
WHERE strCustomerName = 'NATIONWIDE SPECIALTY FINANCE INC' AND strMonthKey = '202601';

SELECT COLUMN_NAME, DATA_TYPE FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMain' AND COLUMN_NAME LIKE '%tangible%';
