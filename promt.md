SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the method TryMergeTtmIntoSeries. Show unified diff BEFORE applying. Do not run build.

BUG FOUND: In TryMergeTtmIntoSeries, the dict[...] KEYS are inconsistent. Some use the canonical "cur" prefix, others don't. The frontend reads canonical cur*-prefixed keys (e.g. curInterestExpenseTTM), but the merge writes non-prefixed keys (e.g. "InterestExpenseTTM"), so the frontend never picks them up — Interest Expense TTM shows $0 even though tblMainTTMCalculations has 387000.

FIX: Change the dict KEY names to the canonical cur*-prefixed form for the 4 that are wrong. Only change the dict KEY (left side); keep the Read(...) arguments (right side, the actual DB column names) exactly as they are.

EXACT CHANGES:
1) dict["InterestExpenseTTM"] = Read(...)   ->   dict["curInterestExpenseTTM"] = Read(...)
2) dict["DepreciationTTM"] = Read(...)      ->   dict["curDepreciationTTM"] = Read(...)
3) dict["AmortizationTTM"] = Read(...)      ->   dict["curAmortizationTTM"] = Read(...)
4) dict["DistributionsTTM"] = Read(...)     ->   dict["curDistributionsTTM"] = Read(...)

Keep these unchanged (already correct):
- dict["curProfitBeforeTaxesTTM"], dict["curCPLTDTTM"], dict["curFixedChargesTTM"], dict["curNetChargeOffTTM"], dict["curAveragePrincipalNRTTM"], dict["curAverageGrossNRTTM"]

Do NOT change the Read(...) arguments (the DB column names inside Read are fine). Do NOT change anything else in the method — not the write loop, not the YTD fallback, nothing.

VERIFY BEFORE SHOWING DIFF:
a) Only the 4 dict KEY names got the "cur" prefix; Read(...) args unchanged.
b) All 10 dict keys now use the cur* canonical prefix consistently.
c) No other lines changed.

Show the unified diff. Apply nothing until I confirm.
