Go ahead and apply the proposed minimal fix.

Important requirements:

1. This fix must be GENERIC and apply to ALL customers, not only ADIR INTERNATIONAL LLC.
2. Do NOT hardcode 202412, 202501, or any customer-specific month.
3. The freshly fetched, server-sorted month-key list must be authoritative for determining the latest available month for the selected fiscal year.
4. The existing maxMonthKey value should only be used as a fallback when the freshly fetched month list is empty/unavailable.
5. Apply only the minimal one-line change identified at line 733 in frontend/src/app/blackbook/edit/page.tsx:
   
   Change the default month selection logic so that when monthKeyParam is not used:
   
   - First prefer the last item from the freshly fetched `arr` (the latest month for the selected fiscal year).
   - Only fall back to `mk`/maxMonthKey when the list does not contain a valid latest month.

6. Preserve the existing behavior when `selectedMonthKey` is already explicitly provided.
7. Preserve the existing `monthKeyParam` behavior.
8. Do NOT modify the legacy BCAT formulas.
9. Do NOT modify fiscal-year calculation logic.
10. Do NOT modify the month list itself, its ordering, dropdown options, or any database/query logic.
11. Do NOT add or remove any UI rows/components.
12. Do NOT change Add New Month behavior.
13. Do NOT refactor unrelated code.

Expected behavior:
- For every customer, after the month-key list is freshly loaded, the latest month available in that selected fiscal year should automatically become the selected Month.
- Example: if the fresh list is [..., 202411, 202412], the initial Month must be 202412.
- If another customer has [..., 202408, 202409], the initial Month must be 202409.
- If maxMonthKey is stale but the fresh list contains a newer valid month, the fresh list must win.
- If the fresh list is empty, existing maxMonthKey fallback behavior should remain.

After applying:
1. Run the appropriate build/type-check/lint validation.
2. Verify there are no TypeScript errors.
3. Verify the change is limited to the intended logic.
4. Show me the exact diff before considering the fix complete.

Do not make any other changes.


SELECT
    c.strCustomerName,
    c.intFiscalYearMonthStart AS fiscal_start_month,
    m.strMonthKey,
    m.intFiscalYear AS stored_fiscal_year,
    m.intFiscalMonth AS stored_fiscal_month,
    YEAR(CONVERT(date, m.strMonthKey + '01')) AS calendar_year,
    MONTH(CONVERT(date, m.strMonthKey + '01')) AS calendar_month,
    CASE
        WHEN MONTH(CONVERT(date, m.strMonthKey + '01')) >= c.intFiscalYearMonthStart
            THEN YEAR(CONVERT(date, m.strMonthKey + '01')) + 1
        ELSE YEAR(CONVERT(date, m.strMonthKey + '01'))
    END AS expected_fiscal_year,
    CASE
        WHEN MONTH(CONVERT(date, m.strMonthKey + '01')) >= c.intFiscalYearMonthStart
            THEN MONTH(CONVERT(date, m.strMonthKey + '01')) - c.intFiscalYearMonthStart + 1
        ELSE MONTH(CONVERT(date, m.strMonthKey + '01')) + 12 - c.intFiscalYearMonthStart + 1
    END AS expected_fiscal_month
FROM tblCustomer c
INNER JOIN tblMain m
    ON LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(c.strCustomerName))
WHERE c.strCustomerName = 'ADIR INTERNATIONAL LLC'
  AND c.strStatus = 'Active'
  AND m.strMonthKey IN ('202411', '202412', '202501', '202502')
ORDER BY m.strMonthKey;
