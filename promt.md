Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

ROOT CAUSE (confirmed): In MatrixCount and MatrixCommitment, the data rows 
and the Totals/Changes/%Change rows each have wrap={false}. Since these are 
single-line rows, wrap={false} is unnecessary to prevent splitting — but it 
forces React-PDF to push a whole row to the next page when it doesn't fit at 
the bottom, creating whitespace (seen on Sample 354/374). Small samples 
(356) fit on one page so don't show it.

FIX: Remove wrap={false} from the single-line DATA rows and the three SUMMARY 
rows (Totals, Changes, % Change) in BOTH matrices, so rows flow naturally and 
fill the page without pushing a whole row down. KEEP wrap={false} on the 
HEADER row (with its minPresenceAhead: 28) so the header stays intact and 
doesn't get orphaned.

In BOTH MatrixCount and MatrixCommitment:

1. DATA rows — currently:
   rows.map(...) => <View style={[styles.tr, ...]} wrap={false}>
   Change to remove wrap={false}:
   rows.map(...) => <View style={[styles.tr, ...]}>

2. Totals row — currently:
   <View style={[styles.tr]} wrap={false}>
   Change to:
   <View style={[styles.tr]}>

3. Changes row — currently:
   <View style={[styles.tr]} wrap={false}>
   Change to:
   <View style={[styles.tr]}>

4. % Change row — currently:
   <View style={[styles.tr, styles.trLast]} wrap={false}>
   Change to:
   <View style={[styles.tr, styles.trLast]}>

5. HEADER row — KEEP wrap={false} exactly as-is:
   <View style={[styles.tr, styles.trHeader]} wrap={false}>   (unchanged)
   (styles.trHeader keeps minPresenceAhead: 28 — do not change)

CONSTRAINTS:
- Remove wrap={false} ONLY from the data rows and the three summary rows 
  (Totals/Changes/%Change) in BOTH matrices.
- KEEP wrap={false} on the header row in both matrices (header integrity).
- Do NOT change row contents, styles, colors, labels, the Changes/%Change 
  calculations, data, or the matrix root (already a plain View).
- Do NOT change the section order or the <View break /> markers between 
  sections.
- Do NOT touch pageSetup.ts, page size, margins, footer, or backend.
- Only edit this one file. Show the changed rows (wrap removed) and confirm 
  the header row still has wrap={false} in both matrices.
