READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

For Item 2, I need to apply fontSize 9 ONLY to the two matrices (MatrixCount, MatrixCommitment), not to shared thDark/td used elsewhere. Show me ONLY (no edits):

1. In MatrixCount and MatrixCommitment: the exact JSX for the blue column-header cells (the row with BANK PD, 1-16, CAS PD Totals headers). Show the full style array on those header Text/View cells (e.g. [styles.thDark, styles.thCompact, {...}]).

2. In both matrices: the exact JSX for the value/body cells and totals cells. Show the full style array (e.g. [styles.td, {...}, styles.tdClamp]).

3. Confirm: is `thCompact` used ONLY by these two matrices (not by detail/subreports)? And is there any matrix-only value style, or do values rely on shared `td`?

4. Show the current fontSize source for: matrix header (thCompact=10?), matrix values (td=10 + inline?).

Read once. Findings only. No edits. I want to find the matrix-only insertion point so shared thDark/td stay untouched.
