READ-ONLY. No edits. Read the FULL dropdown flow in ONE pass. Quote everything below with file paths. This is the ONLY read I will ask — capture all of it.

GOAL: The ConsumerFinance % dropdown (Principal N/R / Gross N/R) next to Cash Collections %, 60+ DPD %, Net C/O % must be INTERACTIVE — switching it recomputes the %. Values are now correct on load but the dropdown no longer changes the value on switch. Find the complete chain and everything needed to fix it in one shot.

All in frontend/src/app/blackbook/edit/page.tsx unless noted. Quote verbatim:

1) The dropdown JSX: the <Select> for Principal/Gross, its value/onChange, and how principalGrossByLabel and principalGrossOptions are declared (useState) and populated. Quote all of it.

2) The tile render block where effectiveVal is computed and where computeConsumerFinancePercentOverride is called — quote the FULL surrounding block including the isConsumerFinance/kind==="percent" guard and exactly which values object is passed (latestTrimmed?.values vs latestPointComputed?.values vs row values).

3) The COMPLETE definitions of latestPointComputed AND latestTrimmed (whichever is passed to the override). Show how each is built and whether the object contains these raw fields: curPrincipalNRPriorMonth, curGrossNRorARPriorMonth, curPrincipalNR, curGrossNRorAR, curAveragePrincipalNRTTM, curAverageGrossNRTTM, curCashCollections, cur60DPD, curNetChargeOff. For EACH of these 9 fields, state present or MISSING in that object.

4) computeConsumerFinancePercentOverride (current, full) + the helpers pick, pickMonthly, cfResolveBasisSelection, and the alias constants CF_PRIN_NR_PRIOR_ALIASES, CF_GROSS_NR_PRIOR_ALIASES, CF_PRIN_NR_ALIASES, CF_GROSS_NR_MONTH_ALIASES, CF_AVG_PRIN_TTM_ALIASES, CF_AVG_GROSS_TTM_ALIASES, CF_CASH_COLL_DOLLAR_ALIASES, CF_DPD60_DOLLAR_ALIASES, CF_NET_CO_DOLLAR_ALIASES. Quote all.

5) How cfResolveBasisSelection maps a tile label to its selection: it reads selMap[`${base} %`] || selMap[`${base} $`]. Quote it. Then quote what KEYS principalGrossByLabel actually uses — when the user changes the dropdown for "Cash Collections %", under what key is the selection stored in principalGrossByLabel? Does that key match what cfResolveBasisSelection looks up (`Cash Collections %` or `Cash Collections $`)? This mismatch could be why switching does nothing.

6) Is effectiveVal (with the override) actually what gets rendered for the tile, or is there a later step that overwrites it? Quote the final value used in the tile's displayed output.

OUTPUT — answer all as a table/list:
- A) dropdown state keys (what principalGrossByLabel uses as keys) vs what cfResolveBasisSelection looks up. Do they MATCH? (This is the prime suspect.)
- B) For the object passed to the override: which of the 9 fields are PRESENT vs MISSING (especially curPrincipalNRPriorMonth / curGrossNRorARPriorMonth).
- C) Does changing principalGrossByLabel actually flow into the override (is the override recomputed on dropdown change, i.e. is principalGrossByLabel a dependency / passed in)?
- D) The single most likely reason the dropdown does nothing now: (i) selection key mismatch in cfResolveBasisSelection, (ii) PriorMonth fields missing from the values object so denom=null→return null, or (iii) effectiveVal overwritten later.
- Quote the exact lines proving your conclusion.
- No fix. Findings only. One pass, then stop.
