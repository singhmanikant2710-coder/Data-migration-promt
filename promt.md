Do NOT modify any code or database data yet.

Proceed with Route A, but first complete the read-only validation.

Run the validation SQL you prepared against the actual database for ALL customers and ALL valid MonthKey rows.

I need the actual results, not predictions.

Return these exact outputs:

1. Total number of tblMain rows checked.
2. Total number of rows where stored intFiscalYear/intFiscalMonth MATCH the required END-YEAR calculation.
3. Total number of mismatched rows.
4. Number of affected customers.
5. Mismatch count grouped by intFiscalYearMonthStart.
6. For every customer, show:
   - Customer Name
   - Fiscal Start Month
   - Earliest MonthKey
   - Latest MonthKey
   - Stored FY/FM
   - Calculated FY/FM
   - Number of mismatched rows

Also specifically validate these customers/examples:

ADIR INTERNATIONAL LLC
Start Month = 2
202402 -> FY2025 / FM1
202403 -> FY2025 / FM2
202412 -> FY2025 / FM11
202501 -> FY2025 / FM12
202502 -> FY2026 / FM1
202503 -> FY2026 / FM2

WORLD ACCEPTANCE CORPORATION
Start Month = 4
202603 -> FY2026 / FM12
202604 -> FY2027 / FM1
202605 -> FY2027 / FM2
202606 -> FY2027 / FM3
202607 -> FY2027 / FM4

For January-start customers:
202601 -> FY2026 / FM1
202602 -> FY2026 / FM2
202612 -> FY2026 / FM12

IMPORTANT:
Do not assume the current stored FY/FM values are correct.
Do not use customer-specific exceptions.
Do not hardcode any customer or start month.

The calculation must always use:

strMonthKey + tblCustomer.intFiscalYearMonthStart

using the END-YEAR convention.

After running the validation, STOP.

Do not:
- UPDATE tblMain
- DELETE anything
- INSERT anything
- modify source code
- modify frontend code
- modify the Add New Month logic

Show me the actual validation output and summary first. We will use those numbers to prepare a safe migration script and then make the minimal code change to prevent future incorrect FY/FM values.
