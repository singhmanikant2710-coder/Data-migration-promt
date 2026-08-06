READ-ONLY. Final complete diagnostics. Do not change anything. One thorough 
read so I can then fix each bug with single bounded prompts.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Show me ALL of the following exactly:

=== A. ALL "Totals" / "Bank PD Totals" / "CAS PD Totals" labels ===
Search the ENTIRE file. For BOTH MatrixCount and MatrixCommitment, AND all 
subreports, show every place a label says "Totals", "Bank PD Totals", or "CAS 
PD Totals":
- The right-side COLUMN HEADER (should be "Bank PD Totals" — show current text)
- The bottom ROW LABEL (should be "CAS PD Totals" — show current text)
- Any subreport that still says plain "Totals"
List each with its exact current text so I can see which are done and which 
still say plain "Totals".

=== B. BOTH footnotes (exact current text) ===
Show the current footnote text in MatrixCount and in MatrixCommitment 
separately. (They must differ: Accounts = "number of accounts", Commitment = 
"committed exposure" — show what each currently says.)

=== C. Overlap in bottom Totals row + CAS PD data cells ===
The bottom "CAS PD Totals" row and the CAS PD data cells overlap in places. 
Show, for BOTH matrices:
- The bottom Totals row: each cell's flexBasis width + the value rendered + 
  fontSize (styles.td).
- A data row: the 16 CAS PD cells' flexBasis (4.25%?) + fontSize.
- Confirm whether decimal values (e.g. "$189.1") are too wide for 4.25% cells 
  at fontSize 10, causing overlap. Show the exact widths and fontSize.

=== D. Header vs data row width alignment ===
Confirm the CAS PD grouping header row, the column-labels header row, the data 
rows, and the bottom Totals row ALL use the SAME column widths (so nothing 
misaligns). Show each row's width set for both matrices. Flag any mismatch 
(e.g. header says 4% but data says 4.25%).

=== E. Zero rendering + fmt() ===
Show how zeros currently render in colored cells (both matrices) and the fmt() 
definition. (I need: hide TRUE zero in colored cells, but show small non-zero 
like $40K = $0.04MM.)

=== F. Detail table page placement ===
Show how the "DETAIL" table (CUSTOMER NAME table) is invoked at the document 
level (DetailTablePages), and any <View break /> or wrap={false}/
minPresenceAhead that pushes it to a later page. (Geoff asked why it's on a 
later page.)

Do not edit anything. Show A-F completely with exact current text/widths/
fontSize. After this I will give ONE bounded fix per bug, one at a time. 
Findings only.
