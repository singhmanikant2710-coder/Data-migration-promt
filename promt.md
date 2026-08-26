-- Confirm required source columns actually exist in tblMain with EXACT names
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMain'
  AND COLUMN_NAME IN (
    'curInterestExpense','curProfitBeforeTaxes','curDepreciation','curAmortization',
    'curDistributions','curCPLTD','curNetChargeOff','curPrincipalNR','curAverageGrossNR'
  )
ORDER BY COLUMN_NAME;
