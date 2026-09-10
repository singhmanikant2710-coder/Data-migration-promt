Hi John,

Could you please share your detailed plan for the BCAT UAT and sign-off?

From our side, we are currently planning to target the BCAT UAT sign-off by 15th September. To align our activities accordingly, could you please let us know how many rounds of UAT you are planning to conduct and the expected timeline for each round?

It would be helpful if you could share the plan in detail, including:

- Number of UAT rounds planned
- Expected timeline for each round
- Expected date for completion of UAT
- Target date for UAT sign-off
- Any key dependencies or activities required from our side

This will help us plan and prioritize the remaining BCAT activities accordingly and ensure we are aligned with the overall timeline.

Thanks in advance for sharing your plan.

Best regards,
Manikant


SELECT intFiscalYearMonthStart, COUNT(*) AS customer_count
FROM tblCustomer
WHERE intFiscalYearMonthStart IS NOT NULL
GROUP BY intFiscalYearMonthStart
ORDER BY intFiscalYearMonthStart;

SELECT 
    t.intFiscalYearMonthStart AS fiscal_start_month,
    t.strCustomerName,
    lm.strMonthKey AS latest_month,
    lm.intFiscalYear AS latest_fiscal_year,
    lm.intFiscalMonth AS latest_fiscal_month
FROM (
    SELECT c.intFiscalYearMonthStart,
           c.strCustomerName,
           ROW_NUMBER() OVER (
               PARTITION BY c.intFiscalYearMonthStart 
               ORDER BY c.strCustomerName
           ) AS rn
    FROM tblCustomer c
    WHERE c.intFiscalYearMonthStart IS NOT NULL
      AND c.strStatus = 'Active'     -- sirf active customers
) t
OUTER APPLY (
    SELECT TOP 1 m.strMonthKey, m.intFiscalYear, m.intFiscalMonth
    FROM tblMain m
    WHERE LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(t.strCustomerName))
    ORDER BY m.strMonthKey DESC
) lm
WHERE t.rn = 1
ORDER BY t.intFiscalYearMonthStart;
