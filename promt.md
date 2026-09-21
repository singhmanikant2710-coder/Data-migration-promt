SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME IN ('tblMainCovenants', 'tblMainDisplayCovenants', 'v_ifu_covenants')
ORDER BY TABLE_NAME, ORDINAL_POSITION;

SELECT *
FROM tblMainCovenants
WHERE strCustomerNumber = '84942562'
   OR strCustomerName = 'ATHENS PAPER COMPANY INC';


   SELECT *
FROM tblMainDisplayCovenants
WHERE strCustomerNumber = '84942562'
   OR strCustomerName = 'ATHENS PAPER COMPANY INC';
