Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Add percentage columns to the two "Final PD Distribution" tables (Geoff: 
"Can we add a % OF COUNT / % OF COMMITMENT column like the tables above?").
Same pattern already used in the Migration Totals tables: each row's value ÷ 
the grand total of that column, to 1 decimal place with "%". Rows sum to ~100%.

=== PART A: "% OF COUNT" in Subreport03_DistByCount ===
This table currently has columns: PD | # OF ACCOUNTS. Add a "% OF COUNT" 
column after it.

a) Before the return/mapping, compute the grand total:
   const grandTotalDistCount = (rows || []).reduce((s, r) => s + (r.count || 0), 0);
   (Use the actual count field name on the row — check what property holds 
   the count value, e.g. r.count or r.numAccounts.)

b) Rebalance the 2-column header to 3 columns and add "% OF COUNT" as the 
   last column (right-aligned). Use inline flexBasis so widths sum to 100% 
   (e.g. PD 40% | # OF ACCOUNTS 30% | % OF COUNT 30%). Move styles.tdLast to 
   the new last (%) column.

c) Add the body cell for % OF COUNT with divide-by-zero guard:
   {grandTotalDistCount > 0 
     ? `${((r.count || 0) / grandTotalDistCount * 100).toFixed(1)}%` 
     : "0.0%"}

=== PART B: "% OF COMMITMENT" in Subreport04_DistByExposure (Final PD 
    Distribution Commitment) ===
This table currently has columns: PD | COMMITMENT. Add "% OF COMMITMENT" 
after it.

a) Compute grand total (raw dollars, matching this table's money() column):
   const grandTotalDistCommit = (rows || []).reduce((s, r) => s + Number(r.sumCommitment || 0), 0);
   (Use the actual commitment field name — check the property, e.g. 
   r.sumCommitment or r.commitment.)

b) Rebalance to 3 columns: PD 40% | COMMITMENT 30% | % OF COMMITMENT 30% 
   (right-aligned). Move styles.tdLast to the new last (%) column.

c) Add the body cell with guard:
   {grandTotalDistCommit > 0 
     ? `${(Number(r.sumCommitment || 0) / grandTotalDistCommit * 100).toFixed(1)}%` 
     : "0.0%"}

CONSTRAINTS:
- FIRST show me the current header + body JSX and the row field names for 
  BOTH Subreport03_DistByCount and Subreport04_DistByExposure, so the count/
  commitment field names and current widths are correct before editing.
- Compute each grand total by reducing the rows array (not from another 
  table's total). For commitment use raw dollars (money()), do NOT reuse any 
  $MM total.
- Include the divide-by-zero guard exactly as shown.
- Widths must sum to 100%; header and body must have the same 3 columns in 
  the same order; move styles.tdLast to the new % column.
- Do NOT change the data, existing values, money() formatting, or any other 
  table.
- Do NOT touch pageSetup.ts, page layout, or backend.
- Only edit this one file. Confirm header column count = body column count 
  (3 = 3) for both tables, and list the changes.
