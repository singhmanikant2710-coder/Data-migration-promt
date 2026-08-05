Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

REORDER the sections in CrmPdGradeMigrationPage so the matrices come first 
(each on its own page), then the charts, then the remaining tables. 
Client-approved layout.

CURRENT order inside CrmPdGradeMigrationPage:
  header → spacer → DistCharts → spacer → MatrixCount → MatrixCommitment 
  → spacer → Subreport01/02/03/04 → footer

NEW order:
  header → spacer → MatrixCount → [page break] → MatrixCommitment 
  → [page break] → DistCharts → spacer → Subreport01/02/03/04 → footer

Implementation:
1. Move <MatrixCount ... /> to be the FIRST content section (right after 
   header + spacer, before DistCharts).
2. Add a page break so MatrixCommitment starts on a new page — wrap it:
   <View break>
     <MatrixCommitment ... />
   </View>
3. Add a page break so DistCharts starts on a new page — wrap it:
   <View break>
     <DistCharts ... />
   </View>
4. Keep Subreport01_Count / 02_Commitment / 03_DistByCount / 04_DistByExposure 
   AFTER DistCharts, in their current relative order (they flow after the 
   charts).
5. Keep header and footer as-is (fixed/per-page).
6. Clean up spacers: keep a small spacer between header and MatrixCount, and 
   between DistCharts and the subreports. Remove spacers left over from the 
   old positions that are now redundant.

CONSTRAINTS:
- Keep MatrixCount and MatrixCommitment wrap={false} as-is (each stays intact 
  on its page). Keep the Changes/% Change rows and column labels already added.
- Do NOT change matrix contents, chart contents, subreport contents, colors, 
  %, data, or the calculated rows — ONLY the render order and page breaks.
- Do NOT touch pageSetup.ts, page size, orientation, margins, Detail pages, 
  or backend.
- Only edit this one file. Show the reordered section JSX.
