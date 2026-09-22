SELECT * INTO tblMain_Backup_PreTest_20260922 FROM tblMain WHERE strCustomerName IN
  ('ATHENS PAPER COMPANY INC','CHARTER PIPE LLC','ADIR INTERNATIONAL LLC',
   'JOHN W STONE OIL DISTRIBUTORS LLC','ALAN WIRE COMPANY','ASBURY MANAGEMENT GROUP, INC',
   'FREZ-N-STOR INC','B W I COMPANIES INC');


SELECT strMonthKey, intFiscalYear, intFiscalMonth FROM tblMain
WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strMonthKey IN ('202507','202510');


SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart
FROM tblMain WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strMonthKey='202609';

SELECT strMonthKey, strCovenantActual FROM tblMainCovenants
WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strCovenantName='Min Tangible Net Worth'
ORDER BY strMonthKey DESC;
