Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Bug #182 — two table fixes:

CHANGE 1 (center CAS PD / CAS LGD data): In the scorecard table (page 2), the 
data values for Bank PD and Bank LGD are already centered, but CAS PD and CAS 
LGD data are LEFT-aligned. Center them to match. (The headers are already 
centered — only fix the DATA cell alignment for CAS PD and CAS LGD.)
Find the CAS PD and CAS LGD DATA cells (the row cells, not headers) and add/
change textAlign to "center" so all four columns' data is centered.

CHANGE 2 (Scorecard ID wrap): The Scorecard ID column (~22% width) doesn't wrap 
long values — they overflow/cut off. Apply the same wrap fix we used in the 
memos: add a wrapAnywhere style (wordBreak: "breakAll") to the Scorecard ID 
cell so long IDs wrap within the column.
- If a wrapAnywhere style already exists in this file, apply it to the 
  Scorecard ID cell.
- If not, add: wrapAnywhere: { wordBreak: "breakAll" } to the stylesheet, and 
  apply styles.wrapAnywhere to the Scorecard ID data cell.
- Also, if the Scorecard ID uses any hyphen-zero-width-space injection 
  (like "-\u200b"), remove that injection and rely on wrapAnywhere (same as the 
  memo fix).
Keep the column width (~22%) unchanged — the ID should wrap within the existing 
width, not widen the table.

CONSTRAINTS:
- CHANGE 1: only add textAlign "center" to CAS PD and CAS LGD DATA cells (Bank 
  PD/LGD already centered).
- CHANGE 2: add wrapAnywhere (wordBreak breakAll) to the Scorecard ID cell; 
  remove any hyphen zero-width-space injection on it; keep the width.
- Do NOT change column widths, headers, other columns, or data values.
- Only edit this one file. Show the CAS PD/LGD data cells (centered) and the 
  Scorecard ID cell (wrapAnywhere).
