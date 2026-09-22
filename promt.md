SELECT strCustomerName, strIndustry, intFiscalYearMonthStart,
       strFiscalYearMonthStart, datFiscalYearStart, *
FROM tblCustomer
WHERE strCustomerName IN (
  'BANKERS HEALTHCARE GROUP LLC',
  'KEYSTONE PRIVATE INCOME FUND',
  'NATIONWIDE SPECIALTY FINANCE INC',
  'VERMEER MOUNTAIN WEST INC'
);
