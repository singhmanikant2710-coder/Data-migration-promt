Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Phase 2 for CRM PD Grade Migration report: heading renames + detail table 
column restructure. Make ONLY these edits:

=== HEADING RENAMES (text only) ===

1. Line ~384, Subreport01_Count heading:
   "Migration by From→To (Count)" 
   -> "PD Migration Totals by Account"

2. Line ~406, Subreport02_Commitment heading:
   "Migration by From→To (Commitment)" 
   -> "PD Migration Totals by Commitment"

3. Line ~432, Subreport03_DistByCount column header:
   "COUNT" 
   -> "# of Accounts"
   (Only this column header text. Do NOT change the section heading 
   "Final PD Distribution (Count)" or the data.)

=== DETAIL TABLE RESTRUCTURE (page 6) ===

Context: The DETAIL table currently has columns: CUSTOMER NAME | REVIEW ID | 
BANK PD | CAS PD | DIRECTION | COMMITMENT | FINALIZED/APPROVED.
Geoff wants: (a) Review ID merged into the Customer Name column, (b) the 
separate REVIEW ID column removed, (c) the FINALIZED/APPROVED column removed.

4. CUSTOMER NAME header (line ~883):
   "CUSTOMER NAME" 
   -> "CUSTOMER NAME (REVIEW ID)"

5. CUSTOMER NAME cell rendering (line ~899). Currently:
   <Text style={[styles.td, styles.wCustomer]}>{out(r.customerName)}</Text>
   Change to concatenate the review id in parentheses:
   <Text style={[styles.td, styles.wCustomer]}>{out(`${r.customerName ?? ""} (${r.reviewId ?? ""})`)}</Text>
   (Keep the same style. Only change the displayed value.)

6. REMOVE the REVIEW ID column entirely:
   - Remove the "REVIEW ID" header <Text> (line ~884) in BOTH DetailTableHeader 
     definitions (the diagnostics noted the header appears in two places — 
     remove from both).
   - Remove the REVIEW ID cell <Text> (line ~900) in DetailTableRows.

7. REMOVE the FINALIZED/APPROVED column entirely:
   - Remove the "FINALIZED/APPROVED" header <Text> (line ~889) in BOTH 
     DetailTableHeader definitions.
   - Remove the corresponding FINALIZED/APPROVED cell <Text> in DetailTableRows 
     (find the cell that renders the finalized/approved value, matching the 
     column position).

CONSTRAINTS:
- After removing REVIEW ID and FINALIZED/APPROVED columns, the remaining 
  detail columns are: CUSTOMER NAME (REVIEW ID) | BANK PD | CAS PD | 
  DIRECTION | COMMITMENT. Ensure header and body cell counts still match 
  (same number of columns in header row and body row).
- Do NOT change the concatenation data source — use the existing r.customerName 
  and r.reviewId fields already available on the row.
- Do NOT touch the backend, any query, or any other table/section.
- Do NOT change the "PD Distribution by Scorecard Commitment" chart title 
  (still pending).
- Do NOT modify the "% OF COUNT" / "% OF COMMITMENT" columns — those are 
  Phase 3.
- If removing columns leaves column widths uneven, you MAY adjust the width 
  styles of the remaining detail columns so they fill the row cleanly, but 
  do NOT change widths of any other table.
- Only edit this one file. List every change with its line, and confirm the 
  header column count equals the body column count after edits.
