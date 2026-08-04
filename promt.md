Two-file edit: 
- frontend/src/components/pdf/InitialMemoPDF.tsx
- frontend/src/components/pdf/FinalMemoPDF.tsx

ISSUE (Geoff): In the header, the "Approver:" value (e.g. "HOULDITCH, GEOFFREY") 
wraps onto two lines. Widen the field so a long approver name fits on ONE line.

Current: the header uses styles.headerGrid3 with three equal columns 
(styles.headerCol: flexBasis "33.33%" each). The third column holds Status / 
Reviewer / Approver — its 33.33% width is too narrow for long names.

FIX: Make the three header columns UNEVEN so the third column (Status/
Reviewer/Approver, which holds the longest text) gets more width, taking 
space from the first two columns which hold short values (customer #, IDs, 
dates).

Instead of all three columns using the shared styles.headerCol (33.33%), 
give each column its own flexBasis:
- Column 1 (Customer #/Review ID/Sample): 30%
- Column 2 (Sample Date/Completed/Distributed or Finalized): 30%
- Column 3 (Status/Reviewer/Approver): 40%

Implementation: apply per-column width overrides on the three 
<View style={styles.headerCol}> elements, e.g.:
  <View style={[styles.headerCol, { flexBasis: "30%" }]}>  // col 1
  <View style={[styles.headerCol, { flexBasis: "30%" }]}>  // col 2
  <View style={[styles.headerCol, { flexBasis: "40%" }]}>  // col 3 (Approver)

Widths sum to 100% (30 + 30 + 40). This gives the Approver column ~40% so 
"HOULDITCH, GEOFFREY" fits on one line (Geoff noted his name is the longest 
approver name — a good sizing rule).

CONSTRAINTS:
- Apply the SAME change to BOTH InitialMemoPDF.tsx and FinalMemoPDF.tsx 
  (they are duplicated).
- Only override the flexBasis of the three header columns. Do NOT change 
  styles.headerCol globally (other things may use it), do NOT change 
  HeaderField, labels, or other layout.
- Do NOT touch pageSetup.ts, page size, orientation, or margins.
- Only edit these two files. Show the updated three header column elements 
  in each file.
