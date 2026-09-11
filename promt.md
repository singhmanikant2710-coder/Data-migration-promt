SELECT
    m.strCustomerName,
    m.strMonthKey,
    m.intFiscalYear AS current_fiscal_year,
    m.intFiscalMonth AS current_fiscal_month,
    CASE
        WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
            THEN CAST(LEFT(m.strMonthKey, 4) AS INT) + 1
        ELSE CAST(LEFT(m.strMonthKey, 4) AS INT)
    END AS expected_fiscal_year,
    CASE
        WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
            THEN CAST(RIGHT(m.strMonthKey, 2) AS INT) - c.intFiscalYearMonthStart + 1
        ELSE CAST(RIGHT(m.strMonthKey, 2) AS INT) + 12 - c.intFiscalYearMonthStart + 1
    END AS expected_fiscal_month
FROM tblMain m
INNER JOIN tblCustomer c
    ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName))
WHERE LTRIM(RTRIM(m.strCustomerName)) = 'ADIR INTERNATIONAL LLC'
  AND m.strMonthKey BETWEEN '202402' AND '202502'
ORDER BY m.strMonthKey;
