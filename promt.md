SELECT
    c.strCustomerName,
    c.intFiscalYearMonthStart AS fiscal_start_month,
    lm.strMonthKey AS latest_month_key,
    lm.intFiscalYear AS latest_stored_fiscal_year,
    lm.intFiscalMonth AS latest_stored_fiscal_month,

    CONVERT(varchar(6),
        DATEADD(
            MONTH,
            1,
            CONVERT(date, lm.strMonthKey + '01')
        ),
        112
    ) AS next_month_key,

    CASE
        WHEN MONTH(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        ) >= c.intFiscalYearMonthStart
        THEN YEAR(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        ) + 1
        ELSE YEAR(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        )
    END AS expected_next_fiscal_year,

    CASE
        WHEN MONTH(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        ) >= c.intFiscalYearMonthStart
        THEN MONTH(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        ) - c.intFiscalYearMonthStart + 1
        ELSE MONTH(
            DATEADD(
                MONTH,
                1,
                CONVERT(date, lm.strMonthKey + '01')
            )
        ) + 12 - c.intFiscalYearMonthStart + 1
    END AS expected_next_fiscal_month

FROM tblCustomer c

OUTER APPLY
(
    SELECT TOP 1
        m.strMonthKey,
        m.intFiscalYear,
        m.intFiscalMonth
    FROM tblMain m
    WHERE LTRIM(RTRIM(m.strCustomerName))
        = LTRIM(RTRIM(c.strCustomerName))
    ORDER BY m.strMonthKey DESC
) lm

WHERE c.intFiscalYearMonthStart IS NOT NULL
  AND c.strStatus = 'Active'

ORDER BY c.strCustomerName;
