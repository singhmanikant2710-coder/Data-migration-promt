Bug 191 fix — register DejaVu Sans as a GLYPH-LEVEL FALLBACK so ≥/≤ render correctly WITHOUT changing document spacing. Approach: fontFamily: ["Helvetica", "DejaVuSans"] on base Page styles (metric-neutral — Helvetica stays primary, DejaVu only used for glyphs Helvetica lacks). Font loaded as base64 data-URL (NO network dependency). Show all diffs before applying.

PHASE 1 — add + register the font (do this first, show me, then stop):
1. Obtain DejaVu Sans (SIL OFL licensed, free commercial use). Subset it to a compact set (Latin + ≥ U+2265, ≤ U+2264, and common punctuation/currency/dashes used in these reports) to keep the bundle small. Convert the subsetted TTF to a base64 string.
2. Create a shared font registration in the existing shared PDF setup module (pageSetup.ts, which all PDF components import). Add:
   Font.register({ family: "DejaVuSans", src: "data:font/ttf;base64,<BASE64>" });
   Ensure this runs at module load, BEFORE any pdf() render (getFont throws if unregistered). Place it so every PDF component that imports pageSetup triggers registration.
3. Do NOT change any Page style yet. Just add the font asset + registration. Confirm the base64 is embedded (no external URL/network fetch).
Report: the file changed, approximate base64 size, and confirm registration runs at import time. STOP for my review.

PHASE 2 (after I approve Phase 1) — apply the fallback:
Change fontFamily from "Helvetica" to ["Helvetica", "DejaVuSans"] at the 18 base Page style sites (ReviewPDF, FinalMemoPDF x2, InitialMemoPDF, CrmSummaryPDF, CrmFindingsObservationsPDF, CrmSummaryTablePDF, CrmSamplesSummaryPDF, CrmPdfGradeMigrationPDF, ScorecardResultsPDF, CroProductionSummaryPDF x3, PolicyExceptionsPDF, CovenantsSummaryPDF, CovenantViolationsPDF, NonComplianceCovenantsPDF, ManagementSummaryPDF, ChecklistQuestionnairePDF). Only the base Page/Document style fontFamily. Do NOT touch any other style, spacing, or HtmlRichText.

Do NOT add any npm package. Do NOT use a network URL for the font.
