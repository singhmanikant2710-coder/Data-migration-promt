═══════════════════════════════════════════════════════════════
  BCAT UAT — DEFECT RESOLUTION SUMMARY
  Verified against legacy MS Access (BCAT_be.accdb)
═══════════════════════════════════════════════════════════════

CONTEXT: BCAT was rebuilt from legacy Access → .NET 8 + Next.js +
SQL Server. Most defects trace to differences from that migration.
Every fix verified side-by-side with legacy output.


───────────────────────────────────────────────────────────────
 #26 — Rolling-24 PDF printed only ~12 months
───────────────────────────────────────────────────────────────
ISSUE      : PDF showed ~12 months, spilled to 2 pages.
ROOT CAUSE : Renderer capped fiscal-year sections at 6 rows each.
FIX        : Render 24 months as two stacked 12-month sections
             (latest 12 top, older 12 bottom) on ONE Legal-
             landscape page.
LEGACY     : Matches legacy BlackBookRolling24Months (12+12,
             single page) for AMERICAN CREDIT ACCEPTANCE.


───────────────────────────────────────────────────────────────
 #35 / #31 — Interest Coverage TTM (and TTM metrics) showed 0
───────────────────────────────────────────────────────────────
ISSUE      : Interest Coverage TTM = 0.00x; TTM values blank for
             new months.
ROOT CAUSE : (1) TTM never recomputed on save.
             (2) Backend wrote non-canonical keys
                 (InterestExpenseTTM) but frontend read cur*
                 (curInterestExpenseTTM) — key mismatch.
             (3) Merge only wrote TTM when existing was 0, so
                 stale values were never overwritten.
FIX        : (1) Year-agnostic trailing-12 recompute on save
                 (ROWS BETWEEN 11 PRECEDING AND CURRENT ROW).
             (2) Aligned all TTM keys to canonical cur* names.
             (3) Merge always writes authoritative TTM value.
FORMULA    : EBIT TTM = PBT TTM + Interest Expense TTM
             Interest Coverage TTM = EBIT TTM / Interest Exp TTM
             (TTM = trailing 12 calendar months, year-agnostic)
LEGACY     : ACA (202607): PBT TTM $350,000, Int Exp TTM $387,000,
             EBIT TTM $737,000, Interest Coverage TTM = 1.90x.


───────────────────────────────────────────────────────────────
 #29 — September (non-December) fiscal year broke TTM/YTD
───────────────────────────────────────────────────────────────
ISSUE      : Sept fiscal-year customers (BHG) computed TTM/YTD
             incorrectly.
ROOT CAUSE : Original TTM recompute was locked to the fiscal
             year, didn't span the boundary.
FIX        : Year-agnostic trailing-12 TTM (from #35) crosses
             fiscal-year boundaries correctly.
LEGACY     : BANKERS HEALTHCARE GROUP (FY 2025/3, 202512):
             Interest Coverage TTM = 2.60x, PBT TTM $240,081,
             Int Exp TTM $149,665, EBIT TTM $389,746.


───────────────────────────────────────────────────────────────
 #30 — Cash Collections % / 60+ DPD % wrong; dropdown not live
───────────────────────────────────────────────────────────────
ISSUE      : Percentages wrong; Principal/Gross dropdown didn't
             recompute correctly.
ROOT CAUSE : (1) Selection field dropped in MapMetricPoint (only
                 strcustomfield* strings were preserved).
             (2) Backend used current-month instead of prior-
                 month denominator.
             (3) LIKE 'PRINCIPAL' (no %) didn't match
                 "Principal N/R".
             (4) Dropdown used Avg-TTM denominator for ALL fields
                 (only correct for Net C/O).
             (5) Zero-basis returned null (froze value) instead
                 of 0%.
FIX        : (1) Emit strPrincipalOrGross* selection to UI.
             (2) Per-field denominator:
                   Cash Collections % → prior month
                   60+ DPD %          → current month
                   Net C/O %          → Avg TTM
             (3) LIKE 'PRINCIPAL%'.
             (4) Dropdown interactive with correct per-field
                 denominator.
             (5) Zero-basis returns 0% (legacy parity).
FORMULA    : Cash Coll % = cash / PriorMonth basis
             60+ DPD %   = dpd  / CurrentMonth basis
             Net C/O %   = netCO / Avg-TTM basis
LEGACY     : 1ST FRANKLIN: Cash Collections % = 5.82% (Principal).
             BHG: 0.34% Principal / 0.00% Gross, 60+ DPD 0.17% —
             dropdown now switches live.
NOTE       : Earlier ">100%" on ACA was unrealistic TEST DATA
             (500000/100000); real customers match legacy.


───────────────────────────────────────────────────────────────
 #31 — YTD Net C/O and TTM Net C/O %
───────────────────────────────────────────────────────────────
ISSUE      : YTD Net C/O $ = 0; TTM Net C/O % inconsistent.
ROOT CAUSE : Same family as #35 (derived aggregates not
             populated/merged; top strip vs panel used different
             rows).
FIX        : Covered by #35 (TTM recompute + key alignment +
             merge overwrite) and #30 (per-field denominators).
LEGACY     : 1ST FRANKLIN: YTD Net C/O $45,473, TTM Net C/O %
             9.65%, Reserve Coverage 0.77x. (ACA showed 0 only
             because that was test data.)


╔═══════════════════════════════════════════════════════════════╗
║ ★★★ BHG MONTH DROPDOWN — Non-December FY: 2026 months showed ║
║     202512's data (HIGHLIGHTED)                              ★★★║
╠═══════════════════════════════════════════════════════════════╣
║ ISSUE                                                          ║
║   For BANKERS HEALTHCARE GROUP (October fiscal year), the      ║
║   Month dropdown listed months up to 202605, but selecting     ║
║   ANY 2026 month (202601–202605) showed 202512's data instead  ║
║   of the selected month's data.                                ║
║                                                                ║
║ ROOT CAUSE                                                      ║
║   GetCurrentYearSeriesAsync (the series feeding the edit-      ║
║   screen tiles) filtered by CALENDAR year:                     ║
║       WHERE LEFT(strMonthKey,4) = @yr                          ║
║   For BHG, fiscal year 2025 spans calendar months              ║
║   202510–202609, so a calendar filter for "2025" returned      ║
║   only 202501–202512. The selected 2026 month was NOT in the   ║
║   series, so the row lookup fell back to the last row (202512).║
║                                                                ║
║   Confirmed at runtime (console):                              ║
║     selectedMonthKey = 202605  (correct)                       ║
║     src months       = [202501 … 202512]  (WRONG — calendar)   ║
║     match            = undefined → fell back to 202512         ║
║                                                                ║
║ FIX                                                            ║
║   Made GetCurrentYearSeriesAsync FISCAL-year aware, mirroring  ║
║   GetHistoricYearSeriesAsync: probe for the intFiscalYear      ║
║   column and filter by:                                        ║
║       WHERE intFiscalYear = @yr   (calendar fallback if col    ║
║                                    absent)                      ║
║   Now the series returns the correct fiscal-year months        ║
║   (202510–202605 for BHG); the selected month is found and     ║
║   its data maps to the tiles.                                  ║
║                                                                ║
║ LEGACY / IMPACT                                                ║
║   BHG now shows each selected month's data (202601, 202603,    ║
║   202605). December fiscal-year customers are UNAFFECTED —     ║
║   for them fiscal year = calendar year, so the filter returns  ║
║   the same rows as before.                                     ║
╚═══════════════════════════════════════════════════════════════╝


───────────────────────────────────────────────────────────────
 ROOT-CAUSE PATTERNS (reusable)
───────────────────────────────────────────────────────────────
 1. Derived values (TTM, prior-month) not recomputed on save.
 2. Backend/frontend key-name mismatch — need one canonical cur*.
 3. Formula inputs (selection, prior-month) dropped in
    MapMetricPoint.
 4. Wrong denominators — current vs prior vs Avg-TTM per field.
 5. Calendar-year vs fiscal-year filtering — only bites non-
    December fiscal-year customers (BHG).
═══════════════════════════════════════════════════════════════
