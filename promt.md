SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Show unified diff BEFORE applying. Do not run build. Do not touch other files.

CONTEXT: The PDF must show the SAME trailing-24 months as the UI. The UI Monthly Summary uses rolling24 (trailing-24, up to 24 rows). The PDF currently renders fiscal-year-split sections (seriesYearOnly current-year + historyYearOnly prior-year), which sums to only ~15 for this customer. The PDF already receives a `rolling24` prop (full trailing-24). Client wants all 24 on ONE page. Fix: render rolling24 as ONE single-chunk section (like the current-year grid), and gate the fiscal-year history sections since rolling24 already contains all 24 months.

EXACT CHANGES:

1) Make the main monthly grid render the full trailing-24 `rolling24` instead of current-fiscal-year-only. Find:
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly] : [];
   Change to (use rolling24, sorted ascending by monthKey, as a single chunk):
   const monthlyChunks = (Array.isArray(rolling24) && rolling24.length > 0)
     ? [[...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))]
     : [];
   (This renders all trailing-24 months in one section, matching the UI. Use the existing normMonthKeyInt helper already used in this file for sorting.)

2) Gate the prior-year (FY-1) history section — it's now redundant since rolling24 already includes those months. Find:
   {historyYearOnly.length > 0 && (
   Change to:
   {false && historyYearOnly.length > 0 && (
   (Replace in place — do not duplicate the line. The FY-2 section is already gated with false &&; leave it.)

DO NOT:
- Do NOT change the disabled rolling24 multi-page block (if (false && r24Chunks...)) — leave it disabled; we are NOT using it (it paginates across pages; client wants one page).
- Do NOT change styles, panels, headings for the current grid, or the Page size.
- Do NOT touch backend or any other file.

VERIFY BEFORE SHOWING DIFF (report; don't force):
a) Confirm normMonthKeyInt is defined/available in this file for the sort (it's used elsewhere here). If not, report — do not invent a sort.
b) Confirm monthlyChunks now derives from rolling24 (full trailing-24) and renders as one chunk (monthlyChunks.length stays 1, so no continuation pages).
c) Confirm the current-grid columns (colsYear) can render rolling24 rows (they're the same MetricPoint shape). If the PDF grid needs colsR24 instead of colsYear for rolling24, report which column set matches the UI's monthly table — do not guess.
d) Confirm both FY-1 and FY-2 history sections are now gated (false &&), so only the single trailing-24 section renders.
e) With ~24 rows in one wrap={false} table plus the panels, report the overflow risk (one page vs spill). Just report.

Show the unified diff. Apply nothing until I confirm.
