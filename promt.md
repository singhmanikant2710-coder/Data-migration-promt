Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix new column widths and center all PD/column headers, in BOTH MatrixCount 
and MatrixCommitment.

PART A — Widen the new columns (Issue 2):
Current widths: BANK PD 8% | 16 PD cols 4.5% (72%) | Totals 12% | # Changes 
4% | % Change 4%. The 4% new columns are too narrow.
Change to: BANK PD 8% | 16 PD cols 4% (64%) | Totals 12% | # Changes 8% | 
% Change 8%. (Total: 8 + 64 + 12 + 8 + 8 = 100%.)
Update these widths in BOTH header rows AND all body rows AND the Totals row, 
in BOTH matrices. Also update the grouping header row 1: blank 8% | "CAS PD" 
span 64% (was 72%) | blank 28% (12+8+8).

PART B — Center all column headers (Issues 1, 8):
The PD column headers (1-16) currently default to LEFT align. Center them.
Add textAlign: "center" to:
- All 16 PD column header cells (currently no textAlign)
- Keep BANK PD header centered (already is)
- The "# Changes" and "% Change" headers: change from textAlign "right" to 
  "center" (so headers are centered, matching the others). Keep the BODY 
  cells of # Changes/% Change right-aligned (numbers read better right-aligned).
- Totals header: keep as-is or center (your call — keep right if it looks 
  aligned with right-aligned totals values).

Ensure header fontSize/fontWeight stay identical across ALL header cells 
(they already use styles.thCompact fontSize 7, fontWeight 700 — keep uniform).

CONSTRAINTS:
- Widths must sum to 100% (8+64+12+8+8).
- Apply identically to BOTH matrices, mirroring header and body widths.
- Center PD column headers + # Changes/% Change headers; keep body number 
  cells right-aligned where they already are.
- Do NOT change data, calculations, colors, or labels.
- Do NOT touch pageSetup.ts, page size, margins, footer, or backend.
- Only edit this one file. Show the updated widths and header alignment for 
  both matrices.
