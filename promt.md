READ-ONLY. No edits. Find how UI shows 24 months but PDF shows only 15. Quote, stop.

CONTEXT: The rolling-summary PDF now fits one page but shows only ~15 months (current fiscal year 8 + prior fiscal year 7), because it renders fiscal-year sections (seriesYearOnly + historyYearOnly). But the UI Monthly Summary shows a full trailing-24-month set. The PDF should match the UI: show the same trailing 24 months, not fiscal-year-split.

1) In frontend/src/app/blackbook/edit/page.tsx (or wherever the UI Monthly Summary table gets its rows): which array does the UI table iterate to show all months? Is it rolling24 / rolling24WithEdits (trailing 24) or seriesYearOnly (current FY only)? Quote the prop/array passed to the UI monthly table.

2) In frontend/src/blackbook/pdf/BlackBookPdf.tsx: confirm the PDF receives a `rolling24` prop. Quote the BlackBookPdf props and where rolling24 comes in. Is rolling24 the full trailing-24 array (same as UI), separate from series/seriesYearOnly?

3) Quote the disabled rolling24 render block in BlackBookPdf (the if (false && r24Chunks...) section) fully — this is likely the intended "24 months in one grid" renderer. Show how r24Chunks is built (splitForPages(rolling24, ...)) and how it maps to rows.

4) Confirm: if we render `rolling24` (trailing 24) as ONE section instead of the fiscal-year-split (seriesYearOnly + historyYearOnly + historyYear2Only), would that match the UI's 24 months? What's the row count of rolling24 for this customer (should be up to 24)?

OUTPUT:
- A) UI table's source array (quoted) — rolling24 or seriesYearOnly?
- B) PDF's rolling24 prop (quoted) — is it the full trailing-24?
- C) The disabled rolling24 render block (quoted).
- D) State plainly: to make the PDF match the UI (24 months), should we render rolling24 as one section and drop/gate the fiscal-year-split sections? yes/no + why.
- No fix yet. Findings only.
