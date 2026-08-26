READ-ONLY. No edits. Three related fields show 0 despite inputs being present. Root-cause all three, quote, stop. No loop.

DATA CONFIRMED (tblMain, 202607): curCashCollections=500000, curPrincipalNRPriorMonth=1000000, curNetChargeOff=50000, curNetChargeOffYTD=0, curAveragePrincipalNRTTM=0 (in tblMain), curDiscountDividedByReserve=0.

Legacy formulas (source of truth):
- perCashCollections = curCashCollections / curPrincipalNRPriorMonth  (should be 500000/1000000 = 50%)
- perNetChargeOffTTM = curNetChargeOffTTM / curAveragePrincipalNRTTM
- curNetChargeOffYTD = running SUM of curNetChargeOff within fiscal year

Investigate THREE things:

1) CASH COLLECTIONS % = 0 despite 500000/1000000:
   - Frontend: in monthSummaryRegistry.ts / mapping (mapIndirectAuto) / tblMainCalcs.ts, find where perCashCollections / "Cash Collections %" is computed. Quote it. Does it read curPrincipalNRPriorMonth correctly? Does it check strPrincipalOrGrossCalculationSelectionCashCollection to choose Principal-vs-Gross denominator? Quote the exact formula and the field names it reads. Is the key for prior-month principal "curPrincipalNRPriorMonth" or a different alias that doesn't match?

2) curAveragePrincipalNRTTM = 0 in tblMain (but our fix writes it to tblMainTTMCalculations):
   - Confirm TryMergeTtmIntoSeries merges curAveragePrincipalNRTTM onto the row (we fixed the keys). But does the frontend "TTM Net C/O %" (perNetChargeOffTTM) read curAveragePrincipalNRTTM from the merged value, or from tblMain (which is 0)? Quote the frontend perNetChargeOffTTM computation and which source it reads. Also: is curNetChargeOffTTM being merged and read correctly (same key-prefix issue we just fixed)?

3) YTD Net C/O $ = 0 (curNetChargeOffYTD not populated):
   - In SqlMainRepository RecomputeOtherYtdsAsync: quote it. Does it compute curNetChargeOffYTD as a running SUM of curNetChargeOff? Is it actually CALLED in the save path (quote the call site)? Report whether it writes curNetChargeOffYTD to tblMain. If it computes but the frontend reads a different key, note that.

OUTPUT:
- A) Cash Collections % formula + the exact field/alias it reads for prior-month principal; state why it's 0 (wrong key, missing selection field, or not computed). Quoted.
- B) TTM Net C/O % source for curAveragePrincipalNRTTM — merged value vs tblMain 0; state the break. Quoted.
- C) YTD Net C/O: is RecomputeOtherYtdsAsync called on save + does it write curNetChargeOffYTD? Quoted. State why it's 0.
- For each, name the exact minimal fix location (don't write the fix yet).
- No fix. Findings only.
