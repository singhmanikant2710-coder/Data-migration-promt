INSERT INTO tblCustomer (<same columns as Athens, except PK/identity>)
SELECT <same columns>, 'ZZTEST DELETE ME INC' AS strCustomerName
FROM tblCustomer
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC';
