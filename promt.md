SELECT TABLE_NAME, COLUMN_NAME, DATA_TYPE, CHARACTER_MAXIMUM_LENGTH,
       NUMERIC_PRECISION, COLUMN_DEFAULT
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND (
        (TABLE_NAME = '03_LIBRARY_10_Distribution Parties' AND COLUMN_NAME = 'Recipient_role')
        OR (TABLE_NAME = '02_CORE_02_Reviews'
            AND COLUMN_NAME IN ('Relationship_mgr_number','Portfolio_mgr_number'))
      );


Decisions — apply Batch 1 with these, then continue Batch 2-4 under the
same stop rule. Batch 5 stays diff-only.

B1 — Fix the numerator. perInterestCoverage = PBT + InterestExpense
     (legacy curEBIT); TTM = PBT_TTM + InterestExpenseTTM. Remove
     Depreciation/Amortization from both (4518-4540).

B2 — Yes, include curFixedChargesTTM (4510-4516): CPLTDTTM +
     InterestExpenseTTM only.

B3 — Use a SECOND UPDATE statement, executed right after the main one,
     same connection/transaction, same WHERE clause, that sets only
     perReserveCoverage from the freshly written perDiscountDividedByReserve
     and perNetChargeOffTTM. Remove perReserveCoverage from the main
     statement's sets list.

B4 — Yes. Guards: 4695 -> Has("perDiscountDividedByReserve") &&
     Has("perNetChargeOffTTM"); 4731 -> Has("curProfitBeforeTaxesYTD").
     Note: curNetIncomeYTD does not exist in SQL Server tblMain, so the
     current write never fires.

B5 — 4565 stays as-is. For the other listed fields, do NOT collapse to 0.
     Match Access exactly by splitting the branch:
       WHEN <den> IS NULL THEN NULL
       WHEN <den> = 0 THEN 0
       ELSE <num> / <den>
     Use the real field names (your list has typos: "perNetChargeCoverage",
     "per6QPD"). Skip 4707 perInventoryTurn and 4663 (already correct).

B6 — curOtherB exists (money). Remove the Related Party fallback at
     4427-4441; formula = (TotalLiabilities - SubordinatedDebt) - OtherB.

Final report per batch: files + line ranges, build result, and the SQL
comparison query for Batch 1 fields.
