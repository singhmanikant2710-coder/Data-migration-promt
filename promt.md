READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

For Item 3 (vertical centering), show me ONLY (no edits):

1. MatrixCount — the "CAS PD Totals" row container JSX (the <View style={[styles.tr, ...]}> that holds the CAS PD Totals label + column totals). Show its full style array. Does it have alignItems anywhere?

2. MatrixCommitment — show the row container JSX for BOTH:
   a. the body/data rows (the rows with values), and
   b. the totals row (CAS PD Totals).
   Show their full style arrays. Do any have alignItems?

3. Confirm styles.tr definition (does it set alignItems? diagnostic said no).

4. Confirm styles.tr is SHARED across all tables (so I add alignItems inline on matrix rows, not to tr itself).

Read once. Findings only. No edits. I need to know exactly which row containers to add alignItems:"center" to, matching Geoff's ask (Accounts: only CAS PD Totals row; Commitment: all values + totals).
