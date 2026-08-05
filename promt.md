READ-ONLY. Diagnostics only. Do not change anything.

The client wants to REORDER the sections in CrmPdGradeMigrationPDF.tsx to 
use page space better. Show me the EXACT current render order of all major 
sections within CrmPdGradeMigrationPage (and the document structure) — 
verbatim, from top to bottom:

1. List every section component rendered in order (DistCharts, MatrixCount, 
   MatrixCommitment, Subreport01/02/03/04, Detail table, etc.) with the 
   surrounding <View>/<Page> structure and any spacers between them.

2. Confirm which are on the first <Page> (CrmPdGradeMigrationPage) vs 
   separate pages/DetailTablePages.

3. Show whether MatrixCount and MatrixCommitment each have wrap={false} 
   (they do per earlier diagnostics) — so I know reordering them will keep 
   each on its own page area.

The target order the client wants:
  1. MatrixCount (PD Matrix by Count) - first
  2. MatrixCommitment (PD Matrix by Commitment)
  3. DistCharts (the two bar charts)
  4. Then the remaining blue-header tables (Subreports/Migration Totals) 
     and Detail

Do not edit anything. Just show the current order and structure so I can 
reorder cleanly. Findings only.
