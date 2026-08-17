READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmSummaryPDF.tsx

Two issues found during verification of #182:

ISSUE 1 — Scorecard ID STILL overflows the table cell for some review IDs, 
despite the wrapAnywhere (wordBreak: breakAll) we added. Show:
1. The Scorecard ID cell — its exact style (flexBasis/width ~22%, wrapAnywhere, 
   padding, fontSize). 
2. The cell's container/row — does the row or cell have overflow handling? Is 
   there padding that pushes content beyond the cell (like the load-samples 
   box-sizing issue)?
3. Does the Scorecard ID Text have flexShrink/minWidth that prevents it from 
   staying within the column? Is wordBreak "breakAll" actually applied, or is 
   there a competing style (whiteSpace nowrap, fixed width) overriding it?
4. For a long ID that still overflows — what's the widest it can get? Is the 
   column flexBasis fixed (22%) but the content forcing it wider? Show if the 
   cell can grow beyond 22%.

ISSUE 2 — Bank PD and Bank LGD values show a "K" that renders ABOVE / on top of 
the table row line (the "K" overlaps the row border/gridline). Show:
1. The Bank PD and Bank LGD DATA cells — their Text style (the { textAlign: 
   "center", width: "100%" } we added, plus fontSize, lineHeight, padding).
2. Is there a value formatter adding "K" (like formatting thousands as "1.2K")? 
   Show the formatter for PD/LGD values.
3. The row/cell height and lineHeight — is the "K" (or the value) taller than 
   the row, causing it to overlap the row's top border? Show lineHeight, 
   fontSize, and any row height constraint.
4. Did adding width: "100%" to these cells change their vertical alignment or 
   push text over the border? Compare with a cell that renders correctly.

Do NOT edit. Show: the Scorecard ID cell style + why it still overflows (width/
padding/competing style), and the Bank PD/LGD cell style + the "K" formatter + 
lineHeight causing the overlap. Findings only.
