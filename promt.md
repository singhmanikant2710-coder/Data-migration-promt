Based on your read-only report. Legacy per-industry forms
(frm0XX...MainCurrentEdit / ...CurrentYear) are the source of truth.

PART A — generic infrastructure (all customers):
1. One shared industry normaliser (legacy tokens: Trucking,
   Manufacturing, WholesaleTrade, DirectAuto, IndirectAuto, Factoring,
   MCA, ConsumerFinance, Other, EnergyRelated, ABL, SpecialtyFinance,
   Equipment) used by backend BlackbookSummaryService, edit page, view
   page, registry, monthSummaryProfiles and industryCustomFieldProfiles.
   Tolerate spaces/case/"Industry"/"IndustryType" suffixes.
2. View page must resolve the same mapping as the edit page for all 13
   industries (add the missing cases, incl. indirectauto).

PART B — Indirect Auto = frm005 exactly (fix items 1-11 of your report):
- Give Indirect Auto its own mapping (copy of mapDirectAuto, then
  adjust). Direct Auto must stay byte-identical.
- Backend indirectauto fixed profile: remove A/R $$$ / A/R Turn Days;
  bind "Discount / Reserve" to curDiscountDividedByReserve /
  perDiscountDividedByReserve.
- Month/TTM: rows and sources exactly as frm005 (incl. Avg Principal
  N/R <- curAveragePrincipalNRTTM, Interest Coverage Month + TTM with
  "x"); rows stay visible when NULL ("—").
- Cash & Charge-offs: rows, $ and % columns and sources exactly as
  frm005 (Discount/Reserve, 60+ DPD, Cash Collections, Net C/O, YTD Net
  C/O, Net C/O TTM $ + %, Reserve Coverage as "x").
- Tile row / grid columns and order exactly as frm005; 5 covenant slots;
  no covenant-threshold tiles that frm005 doesn't have.

PART C — READ-ONLY audit, no changes: for each of the other 12
industries, compare legacy frm0XX (tile row, Month/TTM, Cash & Charge-
offs / middle panel, right rail) with our backend profile + edit
mapping + view mapping. One table per industry, differences only
(label, source field, format, missing/extra), with file:line.

Do not change values, calculations or persistence.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
