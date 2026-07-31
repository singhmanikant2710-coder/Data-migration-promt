Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Final batch for the CRM PD Grade Migration report. All confirmed by Geoff. 
One label rename + two new percentage columns. This completes all edits on 
this report.

=== PART 1: Commitment chart label (bug #5, final part) ===

Line ~565, chart title:
   "PD Distribution by Scorecard Commitment" 
   -> "PD Distribution by Commitment"

=== PART 2: Add "% OF COUNT" column to Subreport01_Count 
    (the "PD Migration Totals by Account" table) ===

Percentage per row = that row's count ÷ grand total of all counts, to 1 
decimal place with "%". Rows should sum to ~100%.

a) In the component body, before the return/mapping, compute grand total:
   const grandTotalCount = (rows || []).reduce((s, r) => s + (r.count || 0), 0);

b) Replace the 3-column header (currently w35/w35/w30) with a 4-column 
   header using inline flexBasis:
   <View style={[styles.tr, styles.trHeader]} wrap={false}>
     <Text style={[styles.thDark, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>FROM PD</Text>
     <Text style={[styles.thDark, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>TO PD</Text>
     <Text style={[styles.thDark, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdRight]}>COUNT</Text>
     <Text style={[styles.thDark, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdLast, styles.tdRight]}>% OF COUNT</Text>
   </View>

c) Replace the body row with 4 matching cells (tdLast moves to the new 
   final % cell):
   <Text style={[styles.td, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>{out(r.fromPd)}</Text>
   <Text style={[styles.td, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>{out(r.toPd)}</Text>
   <Text style={[styles.td, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdRight]}>{out(r.count)}</Text>
   <Text style={[styles.td, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdLast, styles.tdRight]}>
     {grandTotalCount > 0 ? `${((r.count || 0) / grandTotalCount * 100).toFixed(1)}%` : "0.0%"}
   </Text>

=== PART 3: Add "% OF COMMITMENT" column to Subreport02_Commitment 
    (the "PD Migration Totals by Commitment" table) ===

Percentage per row = that row's commitment ÷ grand total commitment, to 1 
decimal place with "%". Rows should sum to ~100%.

a) Compute grand total (raw dollars, matching this table's money() column — 
   do NOT reuse MatrixCommitment's total, which is in $MM):
   const grandTotalCommitment = (rows || []).reduce((s, r) => s + Number(r.sumCommitment || 0), 0);

b) Replace the 3-column header with 4 columns:
   <View style={[styles.tr, styles.trHeader]} wrap={false}>
     <Text style={[styles.thDark, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>FROM PD</Text>
     <Text style={[styles.thDark, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>TO PD</Text>
     <Text style={[styles.thDark, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdRight]}>COMMITMENT</Text>
     <Text style={[styles.thDark, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdLast, styles.tdRight]}>% OF COMMITMENT</Text>
   </View>

c) Replace the body row with 4 matching cells:
   <Text style={[styles.td, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>{out(r.fromPd)}</Text>
   <Text style={[styles.td, { flexBasis: "30%", flexGrow: 0, flexShrink: 0 }]}>{out(r.toPd)}</Text>
   <Text style={[styles.td, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdRight]}>{money(r.sumCommitment)}</Text>
   <Text style={[styles.td, { flexBasis: "20%", flexGrow: 0, flexShrink: 0 }, styles.tdLast, styles.tdRight]}>
     {grandTotalCommitment > 0 ? `${(Number(r.sumCommitment || 0) / grandTotalCommitment * 100).toFixed(1)}%` : "0.0%"}
   </Text>

CONSTRAINTS:
- Compute each grand total by reducing the rows array (props do not provide 
  it). For commitment, use raw dollars (this table uses money()); do NOT 
  reuse MatrixCommitment's $MM total.
- Include the divide-by-zero guard exactly as shown (output "0.0%" if the 
  grand total is 0) to avoid NaN.
- Column widths must sum to 100% (30 + 30 + 20 + 20). Header and body must 
  each have the same 4 columns in the same order. Move styles.tdLast to the 
  new final (%) column in both header and body.
- Do NOT change the underlying data, the count/commitment values, the 
  money() formatting on the commitment column, or any other table/section.
- Do NOT touch the backend, any query, or any other file.
- Only edit this one file. List every change made, and confirm that for 
  BOTH tables the header column count equals the body column count (4 = 4).
