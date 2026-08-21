READ-ONLY. Do NOT edit. Investigate table layout only.

After switching memo PDFs to fontFamily "DejaVuSans" (to fix ≥/≤), the built-in
"Account Information" table in the Initial/Final memo now overflows: the
"Commitment" header column is cut off at the right edge, and the Account # value
overlaps into the Scorecard ID column. DejaVuSans is wider than Helvetica, so the
existing fixed column widths no longer fit. This table is defined in the memo
components (NOT HtmlRichText).

Report only, no edits:

1. Find the "Account Information" table in InitialMemoPDF.tsx / FinalMemoPDF.tsx
   (or a shared component they use). Show the table container width and each
   column's width definition (fixed points? flex? %). Show the Account #,
   Scorecard ID, and Commitment columns.

2. Explain the overflow: do the column widths sum to more than the page content
   width? Are widths hardcoded assuming Helvetica's narrower metrics? Is the
   Commitment column clipped because the table exceeds the right margin?

3. Recommend the safest minimal fix so it fits with DejaVuSans without breaking
   other memo tables: e.g. slightly reduce this table's font size, trim column
   widths, use flexible widths, or reduce cell padding. Which is least risky for
   long account numbers / scorecard IDs?

Output: table style code + why it overflows + safest minimal fix. No edits.
