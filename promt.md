STOP — the approach taken (a standalone folder like .rsfm-pag with its own package.json, pageSetup.js, and fonts/ folder) is NOT what we want. This creates a parallel implementation disconnected from the existing CRM report system.

REQUIREMENT: "CRM Summary for Management" (and every other new report) must be built as a NEW FILE inside the EXISTING frontend/src/components/pdf/ folder — exactly like CrmSummaryPDF.tsx, NonCompliantCovenantsPDF.tsx, PolicyExceptionsPDF.tsx already are. It must:

1. Import and reuse the EXISTING shared pageSetup.ts (fonts, PAGE_SIZE, MARGINS, colors) — do NOT create a new pageSetup file.
2. Import and reuse EXISTING shared components (HtmlRichText, softBreakId, formatCurrency, or any other shared helper already used by other CRM report PDFs) — do NOT duplicate these.
3. Follow the EXACT same header/footer pattern as the other CRM reports: navy header bar with title on the left and date on the right (top-right), footer with report name + page number, no logo — mirroring how CrmSummaryPDF.tsx or PolicyExceptionsPDF.tsx do it today.
4. Live in frontend/src/components/pdf/ as a single .tsx file (e.g. ReviewSummaryForManagementPDF.tsx), NOT in a separate folder with its own package.json/build tooling.
5. Be wired into the existing reports/page.tsx routing (toReportId, onGeneratePdf) the same way every other report is — not through any separate/parallel script (sweep.mjs, verify.mjs, patch.mjs are NOT part of our actual codebase or build).

Delete/ignore any standalone exploration folders (.rsfm-cq, .rsfm-cs, .rsfm-pag) — those are not the deliverable. The deliverable is a properly integrated .tsx file alongside the other report components.

Confirm you understand this before proceeding, and show me where in frontend/src/components/pdf/ the new file will live and which existing shared modules (pageSetup.ts, HtmlRichText.tsx, etc.) it will import — before writing the full report logic.
