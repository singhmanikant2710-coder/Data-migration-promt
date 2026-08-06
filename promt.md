READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two remaining issues after the fixes:

ISSUE 1 (green cell still shows "$0"): In MatrixCommitment, a green colored 
cell (Row 9, Col 1) still displays "$0" despite the zero-blanking fix. Show:
1. The current MatrixCommitment data cell value render (the {bg && ... ? "" : 
   fmt(v)} logic). 
2. The exact condition used to blank zeros.
3. IMPORTANT: cells are in $MM (raw / 1,000,000). A tiny nonzero value (e.g. 
   $400,000 = 0.4 in $MM, or even smaller) is NOT === 0 but fmt() may round 
   it to "$0". So the check "v === 0" misses values that DISPLAY as "$0" but 
   aren't exactly 0. Show fmt()'s definition — does fmt(0.0003) return "$0"? 
   Confirm whether the blank condition should check "fmt(v) === '$0'" or 
   "Math.round(v) === 0" instead of "v === 0".

ISSUE 2 (Totals+footnote isolate on page 2 with blank on page 1): Show the 
current structure of MatrixCount:
1. The data rows (mapped), then the Totals+footnote group. 
2. Confirm the Totals+footnote are in a <View wrap={false}> that is a SIBLING 
   after the data rows (so if the group doesn't fit at page 1 bottom, the 
   whole group moves to page 2, leaving page 1's last portion blank).
3. Is there a way to keep the group with the LAST data row(s)? Or is the 
   issue that the matrix is just slightly too tall so the Totals group always 
   spills? Show the table structure so I can decide: (a) reduce what forces 
   the spill, or (b) accept that when the matrix is tall, the group moves 
   together (minimal isolation).

Do not edit anything. Show fmt() definition, the commitment cell blank 
condition, and the Totals+footnote grouping structure. Findings only.
