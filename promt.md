PART D — fix the Part C differences so every industry matches its legacy
frm0XX form. Same rules as Part B. Apply industry by industry; build
after each; STOP and report if any fixed value/calculation would change.

GENERIC RULES (apply to all industries):
1. Direct Auto: frm004 = frm005 face. Fix all DirectAuto items from your
   C-004 table exactly as done for Indirect Auto (sources, Month/TTM,
   Cash & Charge-offs, Reserve Coverage "x", no covenant-threshold
   tiles, rows stay with "—" when NULL).
2. Sources: use the exact legacy control source first in every backend
   fixed-profile candidate list (e.g. Inventory Turn <- curInventoryTurn,
   FCC TTM <- dblFixedChargeCoverageTTM, D/TNW <-
   perDebtDivTangibleNetWorth, Total N/R <- curPrincipalNR, Net C/O $ <-
   curNetChargeOff, Discount/Reserve <- curDiscountDividedByReserve).
3. Remove tiles/columns/panel rows that the legacy form does not have
   (Trucking extras, MCA / ConsumerFinance / EnergyRelated / ABL
   registry force-includes, Factoring / Other generic middle-panel extras).
4. Add rows the legacy form has and we miss (MCA Avg Principal N/R + Loss
   Reserve %, ConsumerFinance Avg Principal/Gross N/R TTM in Month/TTM,
   Factoring frm006 middle panel, CPLTD <- curCPLTDTTM where legacy shows TTM).
5. Labels exactly as legacy (Factoring, MCA, ConsumerFinance items).
6. ABL custom slots 1..10.
7. Column ORDER: change only where the legacy grid order is certain
   (header layout, not tabIndex). Otherwise leave and list it.
8. Every row/tile: NULL -> "—", never dropped, never $0.

Do not change calculations, stored values or persistence.
Build, tests, do not commit.

REPORT FORMAT (mandatory, per industry):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL case
- NOT TOUCHED (incl. any order item left unchanged and why)
