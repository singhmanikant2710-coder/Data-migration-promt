Fix the "Account Information" table overflow in the CASRR memo PDFs caused by the
DejaVuSans font being wider than the old Helvetica.

CONTEXT (already done, do NOT touch): DejaVuSans is registered and used
(fontFamily "DejaVuSans") in InitialMemoPDF.tsx, FinalMemoPDF.tsx, and
ReviewPDF.tsx (CAS Linesheet). ≥/≤ now render correctly everywhere. Font setup is
complete.

REMAINING BUG (in all three): the 8-column "Account Information" table overflows
because DejaVuSans is wider. The "Commitment" header/column is clipped at the
right edge, and long Account # values overlap into the Scorecard ID column.
Columns use flexBasis percentages summing to 100%, and per-cell padding (6pt each
side) + borders push past the ~540pt Letter-portrait content width.

Edit these files (whichever actually contain this Account Information table):
  - frontend/src/components/pdf/InitialMemoPDF.tsx
  - frontend/src/components/pdf/FinalMemoPDF.tsx
  - frontend/src/components/pdf/ReviewPDF.tsx   (CAS Linesheet)
FIRST confirm the table exists in ReviewPDF.tsx with the same cols8Accounts /
th / td pattern. If ReviewPDF uses a shared table component instead, tell me
where, and fix it there once. Apply the SAME changes consistently. Auto-approve
OFF. Do not touch other tables, shared tokens, or the font setup.

Do exactly this in each file's Account Information table:

1. Add two table-specific styles (leave existing th/td untouched for other tables):
     thAcct: same as th but padding: 4 (not 6)
     tdAcct: same as td but padding: 4 (not 6)

2. In the Account Information table ONLY, replace every styles.th with
   styles.thAcct and every styles.td with styles.tdAcct — header row, ALL data
   rows, and empty-state row. CRITICAL: REPLACE each cell, never duplicate it.
   Exactly one <Text> per cell (no leftover styles.td next to styles.tdAcct).

3. Account # column (c1): apply styles.wrapAnywhere to its cells and remove any
   wrap={false} on Account # cells, so long digit-only account numbers wrap
   instead of bleeding into Scorecard ID. (Scorecard ID c2 already uses
   wrapAnywhere — keep it.)

4. Free horizontal slack so Commitment isn't clipped: change cols8Accounts
   c3, c4, c5, c6 from flexBasis "6%" to "5%" each. Keep c1 21%, c2 29%, c7 13%,
   c8 13%. New total = 96%.

After editing, verify per file: no duplicate <Text> cells; styles.wrapAnywhere
exists (if missing in any file, tell me before using it); tsc has no new errors.
Show all diffs for review — do NOT auto-apply.
