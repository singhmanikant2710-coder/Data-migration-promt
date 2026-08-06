READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two layout refinements needed for MatrixCount and MatrixCommitment:

ISSUE 1 — Totals/Changes/%Change rows split across pages. They should stay 
together as one block.
Show:
1. The current rendering of the Totals row, Changes row, and % Change row in 
   both matrices — are they three separate sibling <View> rows (no grouping 
   parent), each without wrap={false}? Confirm they can currently split 
   across a page boundary.
2. Whether wrapping these three rows in a single <View wrap={false}> parent 
   would keep them together (moving all three to the next page if they don't 
   fit). Confirm the three rows' exact JSX so I can group them.

ISSUE 2 — The footer NOTE (CAS PD Rating totals...) placement/spacing.
Show:
1. Where the note currently renders — immediately after the table, inside the 
   matrix root View? Show its exact position and current marginTop/spacing.
2. The spacing between: the Totals block, the note, and the next section 
   (MatrixCommitment / "BY COMMITMENT"). What spacers/margins exist?
3. Confirm the page footer is <View style={styles.footer} fixed> (absolute, 
   bottom) — so content CANNOT be placed below it (React-PDF limitation). 
   The realistic fix is proper spacing (marginTop on the note, spacer before 
   next section), not placing the note below the fixed footer.

Do not edit anything. Show the three summary rows' structure and the note's 
current placement/spacing. Findings only.
