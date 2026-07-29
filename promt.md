Single-file edit only: frontend/src/app/review-history/page.tsx

In the column definition where key === "sampleName" 
(header "SAMPLE / REVIEW NAME"), edit the cellClassName ONLY.

Current:
  cellClassName: "text-slate-800 text-sm font-medium",

Change to:
  cellClassName: "text-slate-800 text-sm",

Remove the font-medium class so the cell renders at normal font-weight (400), 
matching the other columns.

Do NOT change key, header, sortable, or render. Do NOT touch any other 
column, row, header, file, or styling. This is a pure cosmetic un-bold change.
