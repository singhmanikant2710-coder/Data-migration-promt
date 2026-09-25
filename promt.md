Batch 6 follow-up. Legacy shows covenant actuals only in the months
where they exist (quarterly customers: blank in non-quarter months).
Our UI fills NULL months with the previous value via carry-forward.

1. FIX: remove carry-forward for covenant VALUES in display — MinTNW
   (REG:1581, MST:768) and the covenant keys at REG:1274 and
   REG:2210-2242. A NULL actual must render "—". Keep carry-forward for
   anything that is not a covenant value (e.g. MaxAdjustedDebtTNWLimit).
   List every site you change.
2. READ-ONLY: when a new month is created, what does
   SqlCovenantRepository.SeedFromLatestAsync copy into tblMainCovenants
   — strCovenantActual too, or only name/threshold/format? Compare with
   legacy seeding in Form_frm003WholesaleTradeMain.bas 354-456 (quote
   which columns legacy copies). Report only, don't change seeding.

Build, tests, do not commit.


UPDATE tblMainCovenants
SET strCovenantActual = NULL
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey = '202510'
  AND strCovenantName = 'Min Tangible Net Worth';
