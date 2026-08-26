Do NOT apply the previous diff — it introduced a typo: "curCPLTDTMM" (TMM) instead of "curCPLTDTTM" (TTM). That column name is wrong and would break the CPLTD merge.

Make ONLY these 4 changes in TryMergeTtmIntoSeries — change ONLY the dict KEY (left side) to add "cur" prefix. Do NOT change the Read(...) arguments. Do NOT touch ANY other line.

1) dict["InterestExpenseTTM"] = Read("curInterestExpenseTTM", "InterestExpenseTTM", "InterestTTM");
   ->
   dict["curInterestExpenseTTM"] = Read("curInterestExpenseTTM", "InterestExpenseTTM", "InterestTTM");

2) dict["DepreciationTTM"] = Read("curDepreciationTTM", "DepreciationTTM");
   ->
   dict["curDepreciationTTM"] = Read("curDepreciationTTM", "DepreciationTTM");

3) dict["AmortizationTTM"] = Read("curAmortizationTTM", "AmortizationTTM");
   ->
   dict["curAmortizationTTM"] = Read("curAmortizationTTM", "AmortizationTTM");

4) dict["DistributionsTTM"] = Read("curDistributionsTTM", "DistributionsTTM");
   ->
   dict["curDistributionsTTM"] = Read("curDistributionsTTM", "DistributionsTTM");

CRITICAL — leave these EXACTLY as they already are, do NOT modify them at all:
- dict["curProfitBeforeTaxesTTM"] = Read("curProfitBeforeTaxesTTM", "PBTTTM", "curPBTTTM");
- dict["curCPLTDTTM"] = Read("curCPLTDTTM", "CPLTDTTM");   <-- must stay curCPLTDTTM (TTM, not TMM). Do NOT change this line.
- dict["curFixedChargesTTM"] = ...
- dict["curNetChargeOffTTM"] = ...
- dict["curAveragePrincipalNRTTM"] = ...
- dict["curAverageGrossNRTTM"] = ...

Only the 4 lines above change (adding "cur" to the key). Every other line stays byte-for-byte identical. Do NOT introduce "TMM" anywhere — it must be "TTM".

VERIFY BEFORE SHOWING DIFF:
a) Exactly 4 dict keys changed (Interest, Depreciation, Amortization, Distributions), each gained "cur" prefix.
b) curCPLTDTTM line is UNCHANGED and spelled with "TTM" (not "TMM").
c) No Read(...) arguments changed. No other lines touched.

Show the corrected unified diff. Apply nothing until I confirm.
