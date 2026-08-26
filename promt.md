Hi John,

One field clarification on the TTM fix — I've now checked the full database, so here's the precise picture.

Legacy behavior: legacy computes "Average Gross N/R TTM" as the average of a monthly field called curAverageGrossNR.

New application: I searched every table. The monthly curAverageGrossNR field does not exist in the new app's data. What exists in tblMain is:
- curAverageGrossNRTTM (the aggregate/TTM column itself — no monthly source behind it)
- curGrossNRorAR (monthly)
- curGrossNRorARPriorMonth (monthly)
- curGrossReceivablesCurrentMonth (monthly)

So the exact monthly source legacy used isn't present under the same name. I don't want to guess a mapping (e.g. assume curGrossNRorAR is the same thing) because a wrong source would produce a wrong TTM.

My plan (per your earlier rule "if the source doesn't exist, don't calculate it"):
- Leave Average Gross N/R TTM blank for now.
- Populate all the other TTM values that DO have valid sources — Interest Expense, PBT, Net Charge-Off, Depreciation, Amortization, Distributions, CPLTD, Avg Principal N/R. These are the ones driving the defects you reported (Interest Coverage TTM, Net C/O TTM %, Reserve Coverage).

Two questions:
1. OK to leave Average Gross N/R TTM blank for now (I'll track it separately)?
2. If the business needs it, should curGrossNRorAR be treated as the monthly source for Average Gross N/R, or is it a different field? Once you confirm, I'll map it correctly.

This does not block the main fix — proceeding with the other TTM values now.

Thanks,
Manikant
