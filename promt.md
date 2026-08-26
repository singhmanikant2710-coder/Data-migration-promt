SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the method RecomputeTtmCalculationsAsync. Show unified diff BEFORE applying. Do not run build.

CONTEXT: Client (John Halsrud) confirmed the monthly source for "Average Gross N/R TTM" is curGrossNRorAR (labeled AR or NR by template, same column). Earlier we removed curAverageGrossNR because it didn't exist in tblMain. Now add curAverageGrossNRTTM back, computed as AVG(curGrossNRorAR) over the trailing-12 window, matching legacy's Avg(...).

ADD back the curAverageGrossNRTTM computation using curGrossNRorAR as source:

1) In the required[] array, ADD "curGrossNRorAR" (it exists in tblMain, confirmed).

2) In the CTE, ADD this windowed column (same window as the others: PARTITION BY customer, ORDER BY strMonthKey, ROWS BETWEEN 11 PRECEDING AND CURRENT ROW), placed right after the curAveragePrincipalNRTTM line:
   AVG(curGrossNRorAR) OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curAverageGrossNRTTM
   (Ensure correct commas — this becomes the last column before FROM, or add a comma after the previous line if needed.)

3) In the MERGE WHEN MATCHED UPDATE SET, ADD:
   t.curAverageGrossNRTTM = s.curAverageGrossNRTTM,
   (place it among the other SET lines, before t.datCalculationRun = SYSDATETIME())

4) In the MERGE WHEN NOT MATCHED INSERT column list, ADD curAverageGrossNRTTM.

5) In the VALUES list, ADD s.curAverageGrossNRTTM (matching position, so column count stays equal).

Do NOT change anything else. Do NOT add COALESCE on the AVG output (NULL-when-absent must stay).

VERIFY BEFORE SHOWING DIFF:
a) curGrossNRorAR added to required[].
b) CTE now has curAverageGrossNRTTM = AVG(curGrossNRorAR) with the same window; commas valid.
c) MERGE UPDATE SET, INSERT columns, and VALUES all include curAverageGrossNRTTM; INSERT column count == VALUES count.
d) No COALESCE on the AVG output.

Show the unified diff. Apply nothing until I confirm.
