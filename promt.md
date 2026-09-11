SELECT
    c.strCustomerName,
    c.intFiscalYearMonthStart AS fiscal_start_month,
    m.strMonthKey,
    m.intFiscalYear AS stored_fiscal_year,
    m.intFiscalMonth AS stored_fiscal_month,

    CASE
        WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
        THEN CAST(LEFT(m.strMonthKey, 4) AS INT) + 1
        ELSE CAST(LEFT(m.strMonthKey, 4) AS INT)
    END AS expected_fiscal_year,

    CASE
        WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
        THEN CAST(RIGHT(m.strMonthKey, 2) AS INT)
             - c.intFiscalYearMonthStart + 1
        ELSE CAST(RIGHT(m.strMonthKey, 2) AS INT)
             + 12 - c.intFiscalYearMonthStart + 1
    END AS expected_fiscal_month

FROM tblMain m
INNER JOIN tblCustomer c
    ON LTRIM(RTRIM(c.strCustomerName))
       = LTRIM(RTRIM(m.strCustomerName))

WHERE c.intFiscalYearMonthStart IS NOT NULL
  AND c.strStatus = 'Active'
  AND
  (
      m.intFiscalYear <>
      CASE
          WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
          THEN CAST(LEFT(m.strMonthKey, 4) AS INT) + 1
          ELSE CAST(LEFT(m.strMonthKey, 4) AS INT)
      END

      OR

      m.intFiscalMonth <>
      CASE
          WHEN CAST(RIGHT(m.strMonthKey, 2) AS INT) >= c.intFiscalYearMonthStart
          THEN CAST(RIGHT(m.strMonthKey, 2) AS INT)
               - c.intFiscalYearMonthStart + 1
          ELSE CAST(RIGHT(m.strMonthKey, 2) AS INT)
               + 12 - c.intFiscalYearMonthStart + 1
      END
  )

ORDER BY
    c.strCustomerName,
    m.strMonthKey;
