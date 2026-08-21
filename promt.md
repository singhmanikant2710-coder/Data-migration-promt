READ-ONLY DIAGNOSTIC. Do NOT edit/create/delete any file. Report exact code with line numbers only.

CONTEXT: In BlackBookPdf.tsx the monthly summary is hard-capped to 6 rows via MONTHLY_FIRST_ROWS=6 and a single monthlyChunks slice. I need the surrounding rendering + pagination logic before I fix it, so I don't break page breaks or overflow the PDF.

TASK: Open frontend/src/blackbook/pdf/BlackBookPdf.tsx and report ONLY these, each with exact path:line and quoted code:

1) The full block that defines: MONTHLY_FIRST_ROWS, monthlyRowsOnFirstPage, monthlyChunks, and seriesYearOnly. Quote ~15 lines of surrounding context.

2) How monthlyChunks is consumed in the JSX: quote the .map(...) or render loop that turns each chunk into a <Page> / <View> table. Show how a "chunk" becomes rows, and whether each chunk = one PDF page.

3) Any existing multi-chunk pagination pattern in this same file used for OTHER tables (history sections, rolling24, etc.) — i.e. is there already a helper that splits an array into pages of N rows? Quote it. (e.g. a chunk(arr, size) util, or repeated slice with page breaks.)

4) The definition of seriesYearOnly: how is it derived from the incoming props (series / rolling24)? Is it filtered to current year only? Quote the derivation with line numbers.

5) The disabled rolling24 block: quote the `if (false && ...)` section and the R24_FIRST_ROWS logic, so I can see what "rolling 24" was meant to render and whether the intended 24-month view lives there vs. in the monthly grid.

6) The section heading/title text rendered above the monthly grid and above the (disabled) rolling24 grid — quote the exact <Text> labels, so I know which grid the user actually sees labeled as the 24-month summary.

OUTPUT:
- A) Quoted blocks for items 1–6 with line numbers.
- B) State plainly: is the "rolling 24 months" the user expects served by (i) the monthly grid capped at 6, or (ii) the disabled rolling24 block? Base it on the heading text from item 6.
- C) Confirm whether rendering more than 6 rows requires page-splitting (i.e. would 24 rows overflow one page), based on how chunks map to pages.
- D) No fix proposed. Findings only.
