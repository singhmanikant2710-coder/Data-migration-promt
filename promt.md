Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

This is a batch of pure TEXT LABEL and FONT-SIZE changes for the CRM PD 
Grade Migration report. NO data, query, calculation, or table structure 
changes in this batch. Make ONLY these edits:

1. FONT SIZE (all table values -> 10):
   In the styles.td definition (around lines 199-207), ADD:
       fontSize: 10,
   Do not change any other property. This makes all table body cells render 
   at 10pt (they currently inherit 9pt from styles.page). Do NOT change 
   styles.page fontSize (headers/other text must stay as-is).

2. Line ~564, chart title:
   "PD Distribution by Scorecard Count" 
   -> "PD Distribution by Number of Accounts"

3. Line ~595, matrix heading:
   "PD Grade Migration by Scorecard Count" 
   -> "PD Grade Migration by Number of Accounts"

4. Line ~411, column header in Subreport02_Commitment:
   "COMMITMENT (USD)" 
   -> "COMMITMENT"

5. Line ~448, section heading in Subreport04_DistByExposure:
   "Final PD Distribution (Exposure)" 
   -> "Final PD Distribution (Commitment)"

6. Line ~452, column header in Subreport04_DistByExposure:
   "EXPOSURE (USD)" 
   -> "COMMITMENT"

7. Line ~885, DETAIL table header:
   "PD INITIAL" 
   -> "BANK PD"

8. Line ~886, DETAIL table header:
   "PD FINAL" 
   -> "CAS PD"

CONSTRAINTS:
- These are text-string and one fontSize change ONLY. Do NOT change any 
  data, values, query, calculation, column widths, table structure, or 
  other styles.
- Do NOT touch the "PD Distribution by Scorecard Commitment" chart title 
  (line ~565) yet — that label is pending clarification.
- Do NOT touch the "Final PD Distribution (Count)" / "COUNT" header, the 
  "Migration by From/To" headings, the detail CUSTOMER NAME/REVIEW ID/
  FINALIZED columns, or the footer — those are separate upcoming batches.
- Only edit this one file. List every change made with its line.
