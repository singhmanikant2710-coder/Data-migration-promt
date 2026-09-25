After Batch 5, saving Athens Paper 202510 still leaves tblMain
curProfitBeforeTaxesTTM fiscal-year-scoped: 202510 = 452, 202511 = 725
(452+273). The mirror SQL run manually in SSMS returns the correct 5306.

1. Add a Console.WriteLine at the START of the new tblMain mirror
   ("TTM MIRROR RAN for {cust}") and in its catch (ex.Message), and at
   every early return in RecomputePbtTtmAsync and the mirror.
2. READ-ONLY: grep the WHOLE backend for every statement that writes
   curProfitBeforeTaxesTTM (any table, any method, any caller of
   RecomputePbtTtmAsync). Quote file:line and when each runs relative
   to the mirror.
3. Tell me how to confirm the running Bcat.Api process is the new
   build (which project/port to restart).

Apply only the logging. No logic changes.


SELECT * FROM (
  SELECT strMonthKey, curProfitBeforeTaxesTTM,
    SUM(curProfitBeforeTaxes) OVER (ORDER BY strMonthKey
      ROWS BETWEEN 11 PRECEDING AND CURRENT ROW) AS expTTM
  FROM tblMain WHERE strCustomerName = 'MARTIN INCORPORATED'
) x
WHERE strMonthKey BETWEEN '201907' AND '202006'
ORDER BY strMonthKey;
