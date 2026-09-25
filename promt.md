Batch 5 is applied and the app restarted, but after saving Athens Paper
202510, tblMain.curProfitBeforeTaxesTTM is still 452 (tblMainTTMCalculations
and Access both show 5306). Something skips or silently fails.

1. Temporarily log the exception in the catch blocks of
   RecomputePbtTtmAsync and the new tblMain mirror (ILogger or
   Console.WriteLine, include ex.Message). Also log when the mirror's
   srcCols guard returns early, and which column was missing.
2. Log when RecomputePbtTtmAsync exits early (fiscalYear <= 0 or the
   __hasFy probe).
3. READ-ONLY check: list every statement in the save path AFTER
   RecomputeTtmCalculationsAsync that writes curProfitBeforeTaxesTTM to
   tblMain (could overwrite the mirror). Quote file:line.

Apply 1-2, build, tell me exactly what to look for in the console.
Do not change any logic.


SELECT strMonthKey, curProfitBeforeTaxesTTM FROM (
  SELECT LTRIM(RTRIM(strMonthKey)) AS strMonthKey,
    SUM(curProfitBeforeTaxes) OVER (PARTITION BY LTRIM(RTRIM(strCustomerName))
      ORDER BY LTRIM(RTRIM(strMonthKey))
      ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS curProfitBeforeTaxesTTM
  FROM tblMain
  WHERE LTRIM(RTRIM(strCustomerName)) = 'ATHENS PAPER'
) x
WHERE strMonthKey = '202510';
