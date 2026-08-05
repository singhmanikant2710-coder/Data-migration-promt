Perfect — Option B it is. It's a bit more involved than the reorder alone, but definitely doable. I'll do it in two steps so I can verify each cleanly: first the reordering (matrices first, each on its own page, then charts), then I'll add the "Changes" and "% Change" rows to the matrices plus the SM/SUB/DFUL/LOSS column labels to match your file. I'll make sure the new calculated rows tie out correctly before I send it over. Since you built those totals/changes rows originally, if I have a quick question on exactly how a value should calculate, I may check with you — want to get it right.


READ-ONLY. Diagnostics only. Do not change anything.

The client wants to add to the PD Grade Migration matrices (both MatrixCount 
and MatrixCommitment) to match a reference layout. The reference matrix has, 
below the existing Totals row, TWO additional rows: "Changes" and "% Change". 
It also has special column labels for the higher PD ratings: "13 / SM", 
"14 / SUB", "15 / DFUL", "16 / LOSS" (instead of just "13", "14", "15", "16").

I need to understand the current matrix structure and what data is available 
to build "Changes" and "% Change" correctly.

Show me (no edits):

1. The full MatrixCount component (and MatrixCommitment) — the current 
   structure: header row (column labels 1-16 + Totals), data rows, and the 
   Totals row. Show how each is built.

2. For the "Changes" row concept: in a PD migration matrix, "changes" 
   typically means the off-diagonal movement (cells where fromPd != toPd — 
   i.e., an actual grade change, excluding the no-change diagonal). Show what 
   data the matrix has access to (the pairs: fromPd, toPd, count/commitment). 
   Confirm whether we can compute, per TO-PD column: the total of cells where 
   fromPd != toPd (the "changes" in that column), and the diagonal (no-change) 
   value.

3. For "% Change": likely Changes / Column Total (or Changes / grand total). 
   Show what column totals are already computed (the existing Totals row) so 
   I can derive the percentage.

4. The current column header labels (1 through 16) — where they're defined, 
   so I can add the "/ SM", "/ SUB", "/ DFUL", "/ LOSS" suffixes to columns 
   13, 14, 15, 16.

5. Confirm the diagonal detection: in the matrix, is the "no change" cell the 
   one where the row's fromPd equals the column's toPd? (This determines what 
   counts as a "change" vs "no change".)

Do not edit anything. I need to understand the matrix data model before 
adding calculated rows. Findings only.
