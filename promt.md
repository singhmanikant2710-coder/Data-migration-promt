READ-ONLY. Read ONE file only: frontend/src/blackbook/mappings/tblMainCalcs.ts (or wherever perCashCollections is defined). Read ONCE. Do NOT re-read. Do NOT open other files. Quote and stop.

Find the function/expression that computes perCashCollections (Cash Collections %). Quote it verbatim. I need to see: the exact numerator (curCashCollections) and denominator field it reads (curPrincipalNRPriorMonth or curGrossNRorARPriorMonth), and whether it checks strPrincipalOrGrossCalculationSelectionCashCollection.

Confirmed data: curCashCollections=500000, curPrincipalNRPriorMonth=1000000, selection="Principal N/R". So it should compute 500000/1000000 = 50%. It shows 0, so either the denominator key it reads is different from "curPrincipalNRPriorMonth", or that field isn't on the row.

Output only: the perCashCollections formula (quoted) + the exact denominator key string it reads. Then STOP. No other files, no fix.
