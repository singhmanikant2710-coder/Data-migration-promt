Aap Geoff ko humble aur professional way mein ye likh sakte hain:

> Hi Geoff, I’m really sorry to interrupt. Just as a quick reminder, I’ve added the email IDs of five senior members instead of the three I had mentioned earlier. They are the people I’d like to include for the Letter of Appreciation. I believe the appreciation letters are being shared today.

Thank you again for your support and for taking the time to do this. I truly appreciate the gesture.


Proceed with the safest generic solution.

Business rule is CONFIRMED:

Fiscal Month:
IF calendarMonth >= fiscalStartMonth
    THEN calendarMonth - fiscalStartMonth + 1
    ELSE calendarMonth + 12 - fiscalStartMonth + 1

Fiscal Year:
IF fiscalStartMonth = 1
    THEN calendarYear
    ELSE
        IF calendarMonth >= fiscalStartMonth
            THEN calendarYear + 1
            ELSE calendarYear

Use tblCustomer.intFiscalYearMonthStart as the authoritative fiscal start month.

IMPORTANT:
- Do NOT hardcode any customer names.
- Do NOT hardcode any fiscal start months.
- Do NOT use previous-row FY/FM inheritance.
- The same generic calculation must work for every customer and every month.
- The database stored intFiscalYear/intFiscalMonth must agree with the calculated fiscal values.
- The UI must continue reading those stored values; do not create a separate frontend-only calculation.

SCOPE:

1. Correct ALL 255 mismatched rows identified by the validation query.
2. Do NOT modify the 13,456 already-correct rows unnecessarily.
3. Do NOT migrate the 18 orphan/invalid rows where tblCustomer fiscal start is missing/invalid. Leave them untouched and clearly report them.
4. Do not limit the migration to active customers or recent fiscal years. The requirement is to correct ALL validated mismatched historical rows.
5. The 4 Group-A customers with 100% historical mismatches are intentionally included because the confirmed business rule requires their stored FY/FM to be corrected.

SAFETY REQUIREMENTS:

Before any UPDATE:
- Create a complete backup/snapshot of every row that will be changed.
- Include customer, strMonthKey, old intFiscalYear, old intFiscalMonth, calculated/new fiscal year and calculated/new fiscal month in the backup.
- Use a transaction.
- Do NOT execute the UPDATE automatically yet.

FIRST, generate the migration SQL only for review.

The migration SQL must:
- Update ONLY rows where the calculated FY/FM differs from the stored FY/FM.
- Join tblMain to tblCustomer safely using the same customer matching logic used by the validation query.
- Exclude rows with missing/invalid fiscal start month.
- Calculate FY/FM directly from strMonthKey + intFiscalYearMonthStart.
- Never derive the new FY/FM from the previous row.
- Have a rollback script based on the backup/snapshot.
- Include before/after counts.

Then generate the minimal application-code fix for:

SqlMainRepository.UpsertRowAsync around L1082-L1109.

New rows must calculate fiscal year/month from:
strMonthKey + intFiscalYearMonthStart

and must NOT inherit:
previous row FY/FM + 1

The new-row calculation must use exactly the same generic formula as the migration/validation query.

Do not change unrelated code, UI layout, schema, or reporting logic.

After the proposed SQL and code change are shown, provide a validation query that proves:

1. 0 valid rows remain mismatched.
2. The 18 invalid/orphan rows remain excluded.
3. ADIR INTERNATIONAL LLC:
   202502 = FY2026/1
   202503 = FY2026/2
4. WORLD ACCEPTANCE CORPORATION:
   202603 = FY2026/12
   202604 = FY2027/1
5. A January-start customer:
   202601 = FY2026/1
   202602 = FY2026/2
   202612 = FY2026/12
6. New "Add New Month" rows use the same formula and cannot reintroduce the old drift.

IMPORTANT:
Do NOT execute any UPDATE, INSERT, DELETE, MERGE, ALTER or DROP yet.

Only provide:
A. Migration SQL
B. Rollback SQL
C. Minimal UpsertRowAsync code change
D. Final validation SQL
E. Expected validation criteria

Stop after presenting these artifacts for review.
