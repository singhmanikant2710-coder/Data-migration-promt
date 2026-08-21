Edit ONLY InitialMemoPDF.tsx and FinalMemoPDF.tsx. Auto-approve OFF. Output diffs.

The "Account Information" table overflows with DejaVuSans (Commitment column
clipped, long Account # bleeding into Scorecard ID). Root cause: 8 columns'
flexBasis sums to 100%, and per-cell padding (6pt each side = 96pt/row) + borders
push total width past the 540pt page content area. DejaVuSans is wider than
Helvetica so it now clips.

Apply the safest minimal fix to THIS table only (do not touch shared style tokens
or other memo tables):

1. Reduce this table's cell padding: in the th and td styles used by the Account
   Information table, change padding from 6 to 4 (recovers ~32pt/row). If th/td
   are shared with other tables, create table-specific variants (e.g. thAcct/
   tdAcct) with padding 4 and apply only to the Account Information table cells —
   do NOT change padding for other tables.

2. Enable wrapping on the Account # column (c1) so long digit-only account
   numbers break instead of overflowing into Scorecard ID. Apply the same
   wrapAnywhere style (wordBreak: "breakAll") that Scorecard ID (c2) already uses,
   to the Account # cells. Remove wrap={false} from the Account # cells if it
   prevents wrapping.

3. Only if still tight after 1+2: trim the four narrow PD/LGD columns (c3-c6)
   from 6% to 5.5% each to free ~2% slack. Keep Account #, Scorecard ID, Balance,
   Commitment widths unchanged.

Apply the SAME changes to both InitialMemoPDF.tsx and FinalMemoPDF.tsx (they
share this table). Show both diffs. No auto-apply.
