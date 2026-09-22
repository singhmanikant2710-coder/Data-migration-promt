Apply all of the following now, in this exact order, and confirm each
step before moving to the next:

1. Run the restore for Athens 202510's derived fields — but first check
   which of these 9 columns actually exist on tblMain:
   SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS
   WHERE TABLE_NAME = 'tblMain'
     AND COLUMN_NAME IN ('Over30DayPercent','Over30DPPercent','per30DPD',
       'per60DPD','perCashCollections','perIneligiblePercent',
       'perIneligiblesDividedByNetFundsEmployed','perNetChargeOff','perOver30DPD');
   For whichever of these actually exist, restore them to 0 (matching
   their pre-test state) in the same transaction as the rest below. Do
   NOT touch the 5 float-precision-only fields (dblFixedChargeCoverage,
   perCollateralAvailability, perDebtDividedByTangibleNetWorth,
   perGrossProfitMargin, perGrossProfitMarginYTD) — leave those as they
   are. Do NOT touch dblCovenantActual1 (43469196) — that's the intended
   edit.
   Use the transaction-guarded UPDATE you gave earlier (BEGIN TRAN /
   IF @@ROWCOUNT = 1 COMMIT ELSE ROLLBACK) for the core restore fields
   (intElapsedFiscalDays, dblAccountsReceivableTurnDays, curInventoryTurn,
   curFixedChargesTTM, curCashAvailableForFixedChargesTTM,
   dblFixedChargeCoverageTTM, perInterestCoverageTTM, perInterestCoverage),
   plus whichever of the 9 above exist.

2. git checkout -- backend/src/Bcat.Api/Controllers/MainController.cs

3. Apply the Part 2 diff removing the tblMain insert from
   SqlCovenantRepository.SeedFromLatestAsync (the version you gave with
   the explanatory comment).

4. Apply the elapsed-fiscal-days diff in SqlMainRepository.cs
   (UpdateElapsedFiscalDaysForRowAsync switched to intFiscalMonth * 30,
   plus the call-site comment fix).

After all four: run `git status` and `git diff --stat` and show me the
result — I need to see confirmation that exactly the intended files
changed and nothing else. Also re-run the Athens 202510 restore-check
query so I can see the row is back to its correct pre-test values.
