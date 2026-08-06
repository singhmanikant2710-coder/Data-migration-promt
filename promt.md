Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Replace the footnote text only. Apply to BOTH MatrixCount and MatrixCommitment. 
Do NOT change anything else — no styles, no widths, no placement.

The footnote is the italic <Text> sibling after the table (fontSize 8, 
color #475569, italic). Replace its text content with EXACTLY this (same for 
both matrices):

"CAS PD Totals represent the committed exposure reviewed by CAS PD as of the 
review date. Bank PD Totals represent the committed exposure reviewed by Bank 
PD as of the review date. Red cells indicate PD downgrades by CAS; green cells 
indicate PD upgrades. Based on the review population, there may not be Bank PD 
rows for every PD rating (1-16)."

CONSTRAINTS:
- ONLY replace the footnote text string, in both matrices.
- Keep the footnote's existing style (fontSize 8, color #475569, marginTop 4, 
  marginBottom 10, italic, width 100%) unchanged.
- Keep it as a sibling after the table (do NOT move it inside the table).
- Do NOT change widths, data, or anything else.
- Only edit this one file. Show the updated footnote in both matrices.
