READ-ONLY — do not propose or apply any fix.

Below are the EXACT calculated-field expressions from legacy tblMain
(Access table design, Field.Expression). This is ground truth and
supersedes BCAT_Formula.md and any earlier extraction.

[paste full list here — use exact VBA output for curTotalAdjustedLiabilities,
curInventoryTurno, dblGrossAccountsReceivableTurno]

For EACH field, report in one table:
1. Where the new app computes it (file:line verbatim), or "not computed".
2. MATCH / MISMATCH / NOT IMPLEMENTED — compare exactly:
   a. Formula and operand order
   b. Zero-denominator branch (legacy returns 0)
   c. NULL inputs: Access propagates Null (Null + x = Null; Null
      denominator → Null, not 0). Does our code COALESCE to 0?
   d. Int(): VBA Int is FLOOR (Int(-2.5) = -3). Does our code truncate?
   e. Round(): VBA is banker's rounding. If computed in SQL, ROUND is
      half-away-from-zero — flag it.
3. curEBIT and curEBITTTM are NOT calculated fields. Does our app
   compute them anyway? Quote where.
4. Any field our app computes that is NOT in this list — list them.

Report only. Do not propose or apply a fix.
