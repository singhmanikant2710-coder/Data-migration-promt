SELECT strMonthKey, curInterestExpenseTTM, curProfitBeforeTaxesTTM
FROM dbo.tblMainTTMCalculations
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey = '202607';

  READ-ONLY. No edits. Find why Interest Coverage TTM shows 0 even though curInterestExpenseTTM=387000 is populated in tblMainTTMCalculations. Quote exactly, stop.

CONFIRMED: tblMainTTMCalculations now has curInterestExpenseTTM=387000 for the customer/month (our backend fix works). But the UI still shows Interest Coverage TTM = 0.00x. 

Legacy formula: perInterestCoverageTTM = curEBITTTM / curInterestExpenseTTM, where curEBITTTM = curProfitBeforeTaxesTTM + curInterestExpenseTTM.

Trace how the NEW app produces Interest Coverage TTM for display. Quote:

1) Where is curEBITTTM computed? Search backend (SqlMainRepository, TblMainCalcs.cs) and frontend (blackbook/expr/tblMainCalcs.ts, mappings) for "curEBITTTM" or "EBITTTM". Quote every place it's SET/computed. Is it derived as curProfitBeforeTaxesTTM + curInterestExpenseTTM? Or is it read from a column that doesn't exist / is 0?

2) Where is perInterestCoverageTTM (or the "Interest Coverage TTM" display value) computed? Search for "perInterestCoverageTTM", "InterestCoverageTTM", "interestCoverageTtm". Quote the exact formula/read. Does it read curEBITTTM and curInterestExpenseTTM? From tblMain columns, from the merged series values, or computed client-side?

3) In SqlMainRepository TryMergeTtmIntoSeries: confirm it reads curInterestExpenseTTM from tblMainTTMCalculations and merges it onto the series rows. Does it ALSO compute/merge curEBITTTM (PBT TTM + Interest TTM)? Or is curEBITTTM never produced, leaving the numerator 0?

4) The frontend render for "Interest Coverage TTM" (monthSummaryRegistry.ts): quote its render function. Does it read a precomputed field (curEBITTTM / perInterestCoverageTTM) that may be 0/missing, or compute from curProfitBeforeTaxesTTM + curInterestExpenseTTM?

OUTPUT:
- A) Where curEBITTTM is computed (or "never computed"), quoted.
- B) Where Interest Coverage TTM ratio is computed, quoted, with its exact numerator/denominator source.
- C) State plainly: the ratio is 0 because [curEBITTTM is not computed / the ratio reads a 0 column / frontend doesn't derive it from the populated TTM components]. Identify the exact missing link.
- No fix yet. Findings only.
