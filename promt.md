I understand that you cannot access the database because MFA is required.

Do NOT attempt any connection or code change.

Prepare ONE self-contained READ-ONLY SQL query that I can copy directly into my already-authenticated SSMS session.

Requirements:

1. Read from:
   dbo.tblMain
   dbo.tblCustomer

2. Use tblCustomer.intFiscalYearMonthStart as the ONLY source for each customer's fiscal start month.

3. Calculate Fiscal Year and Fiscal Month from strMonthKey using the END-YEAR convention:

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

4. The query must NOT hardcode any customer name or fiscal start month.

5. I need the query to return these result sets:
   - Overall totals: rows checked, rows matching, rows mismatched, customers checked, customers affected.
   - Mismatch count grouped by fiscalStartMonth.
   - One row per customer showing:
       customer
       fiscalStartMonth
       earliestMonthKey
       latestMonthKey
       storedFY
       storedFM
       calculatedFY
       calculatedFM
       mismatchedRows
       totalRows
   - Rows with missing/invalid fiscal start separately.

6. Also include a clearly separated spot-check section for:
   ADIR INTERNATIONAL LLC
   WORLD ACCEPTANCE CORPORATION
   and one dynamically selected January-start customer.

7. IMPORTANT:
   The query must be completely READ-ONLY.
   No UPDATE, INSERT, DELETE, MERGE, ALTER, DROP or permanent/temp table creation.
   Do not modify code or database.

8. Make the output easy to copy/paste back here.

After providing the SQL, STOP.
Do not make any assumptions about the actual mismatch count until I run it.
