Do NOT use the terminal or PowerShell at all. Read the file directly with your file-read capability (open it in the editor / read tool), not via Get-Content.

READ-ONLY: do not edit/create/delete anything. Just open frontend/src/blackbook/pdf/BlackBookPdf.tsx and quote the exact code (line numbers optional — include them only if your read tool shows them naturally).

Report these 6 sections, each as a quoted code block:

1) The block defining MONTHLY_FIRST_ROWS, monthlyRowsOnFirstPage, monthlyChunks, and seriesYearOnly (with ~15 lines of surrounding context).

2) How monthlyChunks is consumed in the JSX — quote the .map()/render loop that turns each chunk into a <Page>/<View> table. Show whether one chunk = one PDF page.

3) Any existing pagination/chunking helper used elsewhere in this file for other tables (history, rolling24) — e.g. a chunk(arr,size) util or repeated slice-with-page-break. Quote it.

4) The derivation of seriesYearOnly from props (series / rolling24) — is it filtered to current year only? Quote it.

5) The disabled rolling24 block — quote the `if (false && ...)` section and R24_FIRST_ROWS logic.

6) The <Text> heading/title rendered above the monthly grid AND above the disabled rolling24 grid.

Then answer:
- A) Is the "rolling 24 months" the user expects served by the 6-capped monthly grid, or by the disabled rolling24 block? (Base it on the heading text in item 6.)
- B) Would 24 rows overflow a single PDF page, i.e. is page-splitting required? (Base it on how chunks map to pages in item 2.)

No fix yet. Findings only.
