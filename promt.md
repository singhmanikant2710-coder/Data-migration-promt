READ-ONLY. Diagnostics only. Do not change anything.

In frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx (and its backend 
service if needed), I need to locate all of the following text labels and 
table structures for a series of upcoming edits. Report each one's exact 
location (file + line/component) WITHOUT editing:

LABELS TO LOCATE:
1. Chart title "PD Distribution by Scorecard Count"
2. Chart title "PD Distribution by Scorecard Commitment"
3. Matrix heading "PD Grade Migration by Scorecard Count"
4. Section heading "Migration by From/To (Count)" 
5. Section heading "Migration by From/To (Commitment)"
6. Column header "COMMITMENT (USD)" in the From/To commitment table
7. Section heading "Final PD Distribution (Count)" + its "COUNT" column header
8. Section heading "Final PD Distribution (Exposure)" + its "EXPOSURE (USD)" 
   column header
9. In the DETAIL table (page 6): the column headers "CUSTOMER NAME", 
   "REVIEW ID", "PD INITIAL", "PD FINAL", "FINALIZED/APPROVED", and the 
   cell rendering for CUSTOMER NAME and REVIEW ID.
10. The footer that shows the small FHN logo + "CAS RiskReview" + "Page X of Y".

ALSO REPORT:
- For all table CELL VALUE text: the shared style (e.g. styles.td) and its 
  current fontSize, and confirm whether ALL tables use the same cell style 
  (so one fontSize change fixes all) or each defines its own.
- Whether the labels/headings are hardcoded strings in this file, or come 
  from the backend service / constants.
- The structure of the two "Migration by From/To" tables (columns) so I can 
  later add a "% OF COUNT" / "% OF COMMITMENT" column.

Do not edit anything. Findings only.
