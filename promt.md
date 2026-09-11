UPDATE tblMain
SET intFiscalMonth = 12
WHERE LTRIM(RTRIM(strCustomerName)) = 'ADIR INTERNATIONAL LLC'
    AND strMonthKey = '202501'
    AND intFiscalYear = 2025
    AND intFiscalMonth = 1;
