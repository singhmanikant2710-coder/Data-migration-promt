ConsumerFinance fix (all CF customers, all surfaces). Evidence:
MIDDLE GEORGIA MANAGEMENT SERVICES INC 202011 — tblMain:
perDiscountDividedByReserve = 0.0959151433085082 (legacy 9.59%),
perNetChargeOffTTM = 0.0702947845804989 (legacy 7.03%).

1. Loan Loss Reserves %: Summary Top Strip already shows 9.59% (correct).
   Cash & Charge-offs panel shows 0.10% — it is not applying the Access
   Percent x100. Use the same value/formatter as the Top Strip.
2. Net C/O TTM %: UI shows 0.29% on every surface; legacy 7.03%.
   Find which field it reads (likely monthly perNetChargeOff or the
   Principal/Gross selector recompute). Every surface must read the
   stored perNetChargeOffTTM x100: Top Strip, Monthly Summary, Cash &
   Charge-offs, Rolling 24, Fiscal YTD, Detail grid, PDF, CSV. The
   Principal/Gross selector may only recompute the three selector cells
   (Cash Collections %, Net C/O %, 60+ DPD %), never Net C/O TTM %.

EVIDENCE & SAFETY (mandatory):
- Quote the frm008 control source for both fields on each surface.
- Scope: ConsumerFinance only. STOP if another industry is affected.
- Regression before/after: MIDDLE GEORGIA 202011 + MARINER 202603
  (Net C/O TTM 8.51%, Reserve Coverage 122.37%) + GRACELAND RENTALS
  202604 (must stay unchanged).
- No customer-specific code. Do not change values, calculations,
  persistence. Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Root cause per item + ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per surface incl. NULL case
- NOT TOUCHED
