Use the END-YEAR fiscal-year convention.

This is the required business rule.

The fiscal year is labelled by the calendar year in which the fiscal year ENDS.

For a customer with intFiscalYearMonthStart = S:

calendarYear = YYYY
calendarMonth = MM

Fiscal Month:
    IF calendarMonth >= S
        fiscalMonth = calendarMonth - S + 1
    ELSE
        fiscalMonth = calendarMonth + 12 - S + 1

Fiscal Year:
    IF S = 1
        fiscalYear = calendarYear
    ELSE IF calendarMonth >= S
        fiscalYear = calendarYear + 1
    ELSE
        fiscalYear = calendarYear

Examples that MUST pass:

ADIR INTERNATIONAL LLC
start month = 2

202402 -> FY2025 / Month 1
202403 -> FY2025 / Month 2
202412 -> FY2025 / Month 11
202501 -> FY2025 / Month 12
202502 -> FY2026 / Month 1
202503 -> FY2026 / Month 2

WORLD ACCEPTANCE CORPORATION
start month = 4

202603 -> FY2026 / Month 12
202604 -> FY2027 / Month 1
202605 -> FY2027 / Month 2
202606 -> FY2027 / Month 3
202607 -> FY2027 / Month 4

For January-start customers:
start month = 1

202601 -> FY2026 / Month 1
202602 -> FY2026 / Month 2
202612 -> FY2026 / Month 12

IMPORTANT:
Do NOT use the existing tblMain.intFiscalYear/intFiscalMonth values as the source of truth if they conflict with this business rule.

The source of truth for calculating FY/FM is:

tblMain.strMonthKey
+
tblCustomer.intFiscalYearMonthStart

However, because intFiscalYear/intFiscalMonth are currently used as DATABASE FILTER KEYS throughout the application, do NOT make a display-only frontend calculation.

Trace all usages first and implement the fiscal calculation consistently at the backend/database query layer and on new-month insertion.

For existing historical rows whose stored intFiscalYear/intFiscalMonth conflict with the required END-YEAR convention:

1. Do NOT silently change them yet.
2. First produce a read-only validation query/report showing:
   customer
   strMonthKey
   intFiscalYearMonthStart
   current stored intFiscalYear
   current stored intFiscalMonth
   calculated fiscal year
   calculated fiscal month
   mismatch flag

3. Show the complete mismatch count and affected customers/rows.

4. Only after validation should any data migration/update be considered.

NEW MONTH REQUIREMENT:

Add New Month must calculate the next MonthKey from the customer's latest existing MonthKey.

It must NEVER derive the new MonthKey/FY from the FY currently selected in the UI.

Example:

World Acceptance:
latest MonthKey = 202606
start month = 4

next MonthKey = 202607
calculated FY = 2027
calculated Fiscal Month = 4

Therefore Add New Month must create 202607 with FY2027 / Month4 even if the user currently selected FY2026 in the dropdown.

After insert/reload, the UI must show the new MonthKey and its correct fiscal year.

Also fix FY dropdown/month dropdown/query filtering so that the values used for filtering are based on the same fiscal calculation and cannot become inconsistent with the displayed Fiscal: YYYY/M value.

Do not hardcode any customer name or fiscal start month.

Before modifying code, provide:
1. Exact root cause.
2. All locations where intFiscalYear/intFiscalMonth are used as filter keys.
3. All locations involved in Add New Month.
4. The read-only validation SQL.
5. Expected mismatch impact.

Then implement the smallest generic solution.
Do not modify unrelated functionality or UI.
