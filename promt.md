Extend the covenant NULL -> "—" rule to the 3 remaining surfaces:
1. DetailGrid.tsx:352
2. BlackBookPdf.tsx (via report/page.tsx 609/620/634)
3. csv.ts — also remove the Min TNW / Min PBT carry-forward at
   csv.ts:187 and :190 (legacy shows blank for months without a value).
Covenant-scoped only. Do not change formatCurrency or values.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
