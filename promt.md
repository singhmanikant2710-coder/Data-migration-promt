SELECT DISTINCT strCovenantName
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202410' AND '202603';

SELECT strCustomFieldDescription1, strCustomFieldDescription2,
       strCustomFieldDescription3, strCustomFieldDescription4,
       strCustomFieldDescription5
FROM tblCustomer
WHERE strCustomerName LIKE 'ATHENS PAPER%';

READ-ONLY. Blackbook PDF column set differs from legacy for ATHENS
(WholesaleTrade):
- Legacy has, we don't: Min Net Income, AmzN %, Suppressed Availability,
  AmzN $ Ineligible
- We have, legacy doesn't: Min Excess Availability $, Min Interest
  Coverage, Max Distribution %, Min Cash Collection %, Other 1 %,
  Max C/O %, Min Liquidity

1. How does BlackBookPdf decide its columns? (registry profile / fixed
   industry list vs customer covenants + custom fields). Quote file:line.
2. How does the edit-page Monthly Summary decide them (payload path)?
   Is it customer-driven?
3. Legacy: which query/report builds the Blackbook PDF columns
   (qryReportBlackBookData*, rpt*)? Quote the column/control sources.
Report only.


SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblCustomer' AND COLUMN_NAME LIKE '%Custom%'
ORDER BY COLUMN_NAME;

SELECT strCustomFieldDescription1, strCustomFieldDescription2,
       strCustomFieldDescription3, strCustomFieldDescription4,
       strCustomFieldDescription5
FROM tblCustomer
WHERE strCustomerName LIKE 'ATHENS PAPER%';
