SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblCustomer'
  AND COLUMN_NAME LIKE '%Covenant%'
ORDER BY COLUMN_NAME;


SELECT strCustomerNumber, strCustomerName,
       strCovenantAName, strCovenantAThreshold
       -- jitne bhi aur letter-columns Step 1 se milein, sab add karo
FROM tblCustomer
WHERE strCustomerNumber = '84942562' OR strCustomerName = 'ATHENS PAPER COMPANY INC';


SELECT *
FROM tblMainCovenants
WHERE strCovenantThreshold = 43469196;
