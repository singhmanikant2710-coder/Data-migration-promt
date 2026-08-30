═══════════════════════════════════════════════════════════════
 Bug #33 — Van Zyverden: Fiscal Year Start July, blackbook
 shows "Fiscal 2026/9"
 Reported by: Pamela Sullivan (8/10/2026)
═══════════════════════════════════════════════════════════════

STATUS: Resolved. The displayed value is correct and now matches
the legacy application. Fixed as part of a general fiscal-year
correction that applies to all customers.

───────────────────────────────────────────────────────────────
 ROOT CAUSE
───────────────────────────────────────────────────────────────
The application was filtering a customer's data by CALENDAR year
instead of FISCAL year. For customers with a non-December fiscal
start (e.g. Van Zyverden = July, and similarly BHG = October),
this caused the wrong set of months to be loaded and, in some
views, the fiscal year/month to appear inconsistent.

While investigating a related fiscal-year issue on BANKERS
HEALTHCARE GROUP (October fiscal start), I implemented a GENERAL
fiscal-year-aware logic — driven by each customer's actual fiscal
start month (intFiscalYear) rather than the calendar year. This
is customer-agnostic, so it corrects the behaviour for EVERY
non-December fiscal-year customer at once, including Van Zyverden.

───────────────────────────────────────────────────────────────
 WHY "2026/9" IS CORRECT (July fiscal start)
───────────────────────────────────────────────────────────────
With a July start, fiscal months map to calendar months as:

   Fiscal Month  1  →  July      (202507)
   Fiscal Month  2  →  August    (202508)
   Fiscal Month  3  →  September (202509)
   Fiscal Month  4  →  October   (202510)
   Fiscal Month  5  →  November  (202511)
   Fiscal Month  6  →  December  (202512)
   Fiscal Month  7  →  January   (202601)
   Fiscal Month  8  →  February  (202602)
   Fiscal Month  9  →  March     (202603)   ← selected month

The selected month is 202603 (March 2026). For a July fiscal
year, March is fiscal month 9, within Fiscal Year 2026 (the
fiscal year is labelled by the year it ENDS: Jul-2025 to
Jun-2026 = FY 2026). So "Fiscal: 2026/9" is exactly correct.

───────────────────────────────────────────────────────────────
 VERIFIED AGAINST LEGACY (BCAT_be.accdb)
───────────────────────────────────────────────────────────────
   Legacy  →  FY: 2026,  Month: 202603
   Legacy month dropdown  →  202507 … 202603 (Jul-2025–Mar-2026)

   New app →  FY: 2026,  Month: 202603,  Fiscal: 2026/9
   New app month dropdown →  202507 … 202603 (identical)

Both applications show the same fiscal year and the same fiscal-
year months. The new application now matches legacy exactly.

───────────────────────────────────────────────────────────────
 IMPACT
───────────────────────────────────────────────────────────────
• Van Zyverden (July FY): fiscal year, months, and data now
  load and display correctly.
• Because the fix is general (fiscal-year-based, not per-
  customer), all non-December fiscal-year customers are
  corrected by the same change.
• December fiscal-year customers are unaffected — for them the
  fiscal year equals the calendar year, so behaviour is
  unchanged.

───────────────────────────────────────────────────────────────
 RESOLUTION
───────────────────────────────────────────────────────────────
Fixed via the general fiscal-year-aware data logic. The "2026/9"
value is correct and legacy-consistent (FY 2026, fiscal month 9
= March for a July start).

Optional (UI clarity only): if the "YYYY/M" format is felt to be
confusing, the label could be enhanced to e.g.
"Fiscal: 2026 / Month 9 (Mar)". This is a display-only change,
not a calculation fix — happy to add it if preferred.
═══════════════════════════════════════════════════════════════
