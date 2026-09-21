SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMain'
  AND (COLUMN_NAME LIKE '%TNW%'
    OR COLUMN_NAME LIKE '%Tangible%'
    OR COLUMN_NAME LIKE '%NetWorth%'
    OR COLUMN_NAME LIKE '%Covenant%'
    OR COLUMN_NAME LIKE '%Threshold%'
    OR COLUMN_NAME LIKE '%Min%')
ORDER BY COLUMN_NAME;

SELECT TOP 20 strCustomerNumber, strCustomerName, strCovenantName, strMonthKey, strCovenantThreshold
FROM tblMainCovenants
ORDER BY strMonthKey DESC;

SELECT DISTINCT strCustomerNumber, strCustomerName, strCovenantName
FROM tblMainCovenants
WHERE strCustomerName LIKE '%ATHENS%';

SELECT TOP 20 strCustomerNumber, strCustomerName, strMonthKey, strCovenantName, strThreshold1
FROM tblMainDisplayCovenants
ORDER BY strMonthKey DESC;

SELECT TOP 20 CustomerId, IndustryCode, MonthKey, CovenantName
FROM v_ifu_covenants
ORDER BY MonthKey DESC;


SELECT strMonthKey, strCovenantName1, strCovenantName2, strCovenantName3,
       strCovenantName4, strCovenantName5, strCovenantName6
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
  AND strMonthKey IN ('202507','202510')
ORDER BY strMonthKey;

SELECT strMonthKey, strCovenantName1, dblCovenantActual1, dblCovenantActual1Formatted
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
  AND strMonthKey IN ('202507','202510')
ORDER BY strMonthKey;
