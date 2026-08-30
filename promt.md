═══════════════════════════════════════════════════════════════
 Bug #33 — Van Zyverden: Fiscal Year Start July, blackbook
 shows "Fiscal 2026/9"
 Reported by: Pamela Sullivan (8/10/2026)
═══════════════════════════════════════════════════════════════

STATUS: Not a defect — the displayed value is correct and
matches the legacy application. Clarification below.

───────────────────────────────────────────────────────────────
 WHY "2026/9" IS CORRECT
───────────────────────────────────────────────────────────────
Van Zyverden has a JULY fiscal-year start. With a July start,
the fiscal months map to calendar months like this:

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
year, March falls in fiscal month 9. So "Fiscal: 2026/9" reads
as "Fiscal Year 2026, fiscal month 9" — which is exactly right.

The fiscal year is labelled by the year in which it ENDS: a
July-2025-to-June-2026 fiscal year is Fiscal Year 2026. That is
why the FY dropdown shows 2026, not 2025.

───────────────────────────────────────────────────────────────
 VERIFIED AGAINST LEGACY (BCAT_be.accdb)
───────────────────────────────────────────────────────────────
Opened Van Zyverden in the legacy Access application for the
same month:

   Legacy  →  FY: 2026,  Month: 202603
   Legacy month dropdown  →  202507 … 202603
                             (July 2025 through March 2026)

   New app →  FY: 2026,  Month: 202603,  Fiscal: 2026/9
   New app month dropdown →  202507 … 202603  (identical)

Both applications show the same fiscal year (2026) and the same
set of months for the fiscal year. The new application matches
legacy exactly.

───────────────────────────────────────────────────────────────
 NOTE ON THE MONTH DROPDOWN
───────────────────────────────────────────────────────────────
A related issue existed where, for non-December fiscal-year
customers, the month dropdown/data only covered the calendar
year (e.g. Jan–Dec) instead of the fiscal year, so later months
of the fiscal year could not be viewed. That has been fixed
separately (the current-year data query is now fiscal-year
aware). Van Zyverden now correctly lists and loads all fiscal-
year months (202507–202603), matching legacy.

───────────────────────────────────────────────────────────────
 RESOLUTION
───────────────────────────────────────────────────────────────
No code change required for the "2026/9" display — it is
correct and legacy-consistent. The fiscal-year label (2026) and
the fiscal month (9 = March for a July start) are both accurate.

If the "YYYY/M" format is felt to be confusing to users, we can
optionally enhance the display for clarity — for example:
   "Fiscal: 2026 / Month 9 (Mar)"
This would be a small UI-label change only, not a calculation
fix. Awaiting confirmation on whether that enhancement is
wanted.
═══════════════════════════════════════════════════════════════
