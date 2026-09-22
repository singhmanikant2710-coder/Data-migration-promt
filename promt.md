SELECT strCustomerName, strIndustry
FROM tblCustomer
WHERE strCustomerName IN ('ATHENS PAPER COMPANY INC','IMPERIAL TRADING CO LLC','NATIONWIDE SPECIALTY FINANCE INC');


SELECT c.strCustomerName, c.strIndustry, c.intFiscalYearMonthStart
FROM tblCustomer c
WHERE c.strIndustry = 'DirectAuto'
ORDER BY c.strCustomerName;
