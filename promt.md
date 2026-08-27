READ-ONLY. Read once, quote, stop. No loop. Find the Cash Collections % DROPDOWN recompute logic.

FACT: Stored perCashCollections = 0.0582 (5.82%, correct, = curCashCollections/curPrincipalNRPriorMonth = 63350/1088142). But the UI Cash Collections % dropdown, when "Principal N/R" is selected, shows 5.98% = 63350/1059729, which is curAveragePrincipalNRTTM (Avg Principal NR TTM), NOT curPrincipalNRPriorMonth. So the dropdown's live recompute uses the WRONG denominator.

In frontend (monthSummaryRegistry.ts / the Cash & Charge-offs panel with the Principal/Gross dropdown, or wherever the dropdown onChange recomputes Cash Collections %):
1) Find the dropdown for Cash Collections % (Principal N/R vs Gross N/R selector). Quote its onChange / recompute logic.
2) When "Principal N/R" is selected, what denominator does it use? Quote the exact field. Is it curPrincipalNRPriorMonth (correct) or curAveragePrincipalNRTTM (wrong)?
3) When "Gross N/R" is selected, what denominator? curGrossNRorARPriorMonth (correct) or curAverageGrossNRTTM?
4) Compare with the correct perCashCollections calc in tblMainCalcs.ts (which uses curPrincipalNRPriorMonth / curGrossNRorARPriorMonth).

OUTPUT:
- A) The dropdown recompute logic, quoted.
- B) The denominator it uses for Principal (and Gross), quoted — is it PriorMonth or AverageTTM?
- C) State plainly: the dropdown uses [curAveragePrincipalNRTTM] instead of [curPrincipalNRPriorMonth], giving 5.98% instead of 5.82%.
- D) Exact fix location.
- No fix yet. Findings only.
