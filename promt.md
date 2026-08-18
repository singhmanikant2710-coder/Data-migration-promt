READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

The MatrixCommitment body values don't vertically align across a row — the diagonal value cells sit higher than the "Bank PD Totals" column values in the same row. I need to center them WITHOUT alignItems on the row (that caused line artifacts). Show me ONLY (no edits):

1. The full MatrixCommitment body row JSX — the row <View> and EVERY child cell (the diagonal/value cells with tdClamp, the empty cells, the Bank PD Totals cell, # Changes, % Change cells). Show the complete style array on each.
2. styles.tdClamp definition (all properties).
3. styles.td definition (padding, lineHeight, any height).
4. The difference between the value cells (which use tdClamp) and the Bank PD Totals / # Changes cells (which may NOT use tdClamp) — this padding/clamp difference is likely why they sit at different vertical positions.

Read once. Findings only. No edits. I want to equalize vertical position via consistent padding/lineHeight per cell, not alignItems.
