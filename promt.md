SELECT strMonthKey, intFiscalYear FROM tblMain
WHERE strCustomerName = "KEYSTONE PRIVATE INCOME FUND"
  AND strMonthKey In ("202104","202110","202510");

  SELECT strMonthKey FROM tblMain
WHERE strCustomerName = "ADIR INTERNATIONAL LLC"
  AND strMonthKey Between "202401" And "202404";
