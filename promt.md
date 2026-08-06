Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix ONLY the MatrixCount footnote text. It currently says "committed exposure" 
but for the Number of Accounts matrix it must say "number of accounts". Do NOT 
touch MatrixCommitment's footnote or anything else.

In MatrixCount (PD Grade Migration by Number of Accounts), replace the footnote 
text with EXACTLY:

"CAS PD Totals represent the number of accounts reviewed by CAS PD as of the 
review date. Bank PD Totals represent the number of accounts reviewed by Bank 
PD as of the review date. Red cells indicate PD downgrades by CAS; green cells 
indicate PD upgrades. Based on review population, there may not be Bank PD rows 
for every PD rating (1-16)."

CONSTRAINTS:
- ONLY change the MatrixCount footnote text (the one after MatrixCount's table).
- Do NOT change MatrixCommitment's footnote (it correctly says "committed 
  exposure" — leave it).
- Do NOT change styles, widths, data, or anything else.
- Only edit this one file. Show the updated MatrixCount footnote.
