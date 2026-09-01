SELECT intFiscalYearMonthStart, strCustomerName
FROM (
    SELECT c.intFiscalYearMonthStart,
           c.strCustomerName,
           ROW_NUMBER() OVER (
               PARTITION BY c.intFiscalYearMonthStart 
               ORDER BY c.strCustomerName
           ) AS rn
    FROM tblCustomer c
    WHERE c.intFiscalYearMonthStart IS NOT NULL
) t
WHERE rn = 1
ORDER BY intFiscalYearMonthStart;
