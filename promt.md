


BEGIN TRAN;
DELETE FROM tblMainCovenants        WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMainDisplayCovenants WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMainTTMCalculations  WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
DELETE FROM tblMain                 WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202604','202605');
-- rows affected check karo (tblMain = 1 ya 2), phir:
COMMIT;

SELECT strCovenantName1, strCovenantName2, strCovenantName3, strCovenantName4, strCovenantName5
FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604';

SELECT strCovenantName, intCovenantOrder, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604'
ORDER BY intCovenantOrder;

New month seeding writes "Other 1 (%)" (intCovenantOrder 0) into a
tblMain covenant slot (ATHENS 202604). Legacy funSave writes
strCovenantName{i} / dblCovenantActual{i} with i = intCovenantOrder, only
for 1..N; order 0 / out-of-range never occupies a slot. Fix the new-month
seeding (SeedCovenantsFromPreviousMonthAsync / SeedFromLatestAsync) to
place each covenant in slot = its intCovenantOrder, 1..N only. Quote
legacy. Generic. Build, tests, do not commit. Report ADDED/REMOVED,
BEHAVIOUR CHANGE incl. NULL, NOT TOUCHED.


SELECT strCovenantName1, strCovenantName2, strCovenantName3, strCovenantName4, strCovenantName5
FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604';

SELECT strMonthKey, curCPLTDTTM, curInterestExpenseTTM, curFixedChargesTTM,
       curCashAvailableForFixedChargesTTM, dblFixedChargeCoverageTTM
FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey IN ('202603','202604');
