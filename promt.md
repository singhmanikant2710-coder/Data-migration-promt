Do NOT touch UpsertRowWithConnectionAsync or any write logic yet. This step
is validation-only.

Fix the fiscal-year formula in DeriveFiscalFromMonthKey to:
fy = (m >= start) ? y + 1 : y
fm = ((m - start + 12) % 12) + 1

(Your previous version had "y - 1" instead of "y + 1" — confirmed wrong
against real data: 201610 → intFiscalYear 2017, 202507 → 2025, 202510 →
2026, all already correct in the database today.)

Then write a read-only validation script/query that:
1. Gets every distinct intFiscalYearMonthStart value currently in tblCustomer
   (every fiscal-start-month our customers actually use, not just October).
2. For EVERY row in tblMain, across EVERY customer, recomputes
   (intFiscalYear, intFiscalMonth) from strMonthKey using the corrected
   formula and that customer's fiscal start month.
3. Compares the recomputed values against what's actually stored in
   intFiscalYear/intFiscalMonth for that row.
4. Reports: total rows checked, mismatch count grouped by
   intFiscalYearMonthStart, and up to 10 example mismatched rows per group
   if any exist.

Run it and report the results. Zero mismatches is required before this
formula is trusted. Do not write or modify any files.
