Net C/O TTM % still shows 0.29% for MIDDLE GEORGIA 202011
(ConsumerFinance) on Top Strip and Cash & Charge-offs.

Evidence:
- rolling24 / current-year API return perNetChargeOffTTM = 0.0702947...
  (correct, legacy 7.03%).
- /summary payload: { id: "fixed.ttmNetCoPercent", label: "Net C/O TTM %",
  format: "percent", source: "perNetChargeOffTTM" } (correct).
- 0.29% = monthly curNetChargeOff / curAveragePrincipalNRTTM, i.e. the
  Principal/Gross selector value of "Net C/O %". So some frontend code
  matches the label by prefix ("Net C/O") and applies the selector
  recompute to "Net C/O TTM %".

1. Find EVERY place that applies the selector/percent override or
   recompute by label (edit/page.tsx computeConsumerFinancePercentOverride
   and callers, MonthSummaryTable payload render, monthSummaryRegistry,
   consumerFinance.ts carry(), Top Strip tile build). Quote file:line.
2. Fix: the selector may affect ONLY exact labels "Cash Collections %",
   "Net C/O %", "60+ DPD %" (exact match, no prefix/regex). "Net C/O TTM %"
   always shows stored perNetChargeOffTTM x100.

   3. The Principal/Gross dropdown's initial value must come from the
   stored strPrincipalOrGrossCalculationSelection* field for that month
   (MIDDLE GEORGIA 202011 = "Principal N/R"), not a default.
   Quote where the default is set.

EVIDENCE & SAFETY: ConsumerFinance only; regression before/after
MIDDLE GEORGIA 202011, MARINER 202603, GRACELAND RENTALS 202604; confirm
"Net C/O %" selector cell still changes with the dropdown.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Root cause (file:line) + ADDED / REMOVED
- BEHAVIOUR CHANGE per surface incl. NULL case
- NOT TOUCHED

