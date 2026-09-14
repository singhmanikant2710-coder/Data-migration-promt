Bug 225 fix. SINGLE FILE, 2 lines. Show diff, do NOT commit.

FILE: frontend/src/components/pdf/CrmSummaryPDF.tsx

1. Line 583 (Scorecard Assessment header row): <View style={[styles.tr, styles.trHeader]}> → append wrap={false}.
2. Line 597 (Scorecard Assessment data row): <View key={`sc-${i}`} style={[styles.tr, i === (scRows.length || 1) - 1 ? styles.trLast : {}]}> → append wrap={false}.

Mirror the exact working pattern in NonCompliantCovenantsPDF.tsx (295, 308-311) and CroProductionSummaryPDF.tsx (406, 418).

Do NOT touch any other table in this file. Do NOT change column widths, softBreakId, or the Scorecard ID wrapping logic.

Show diff. Rebuild. Do NOT commit.
