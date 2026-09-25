Decisions:

6f Mechanism A — option (c). When strCovenantFormat is empty, do NOT
   infer % or x from threshold text or label. Show the bare number with
   2 decimals ("26,207.22"), same as legacy FormatNumber(x,2).

6f Mechanism B — when no slot matches by name, keep the slot's value
   and label but do NOT use the definition's format (treat as empty ->
   bare number). Do not drop the field.

6c follow-up — apply the same gap-fill-only change to the prior-year
   enrichment block in view/page.tsx 846-850, so both grids behave the
   same.

Registry 1574 / 2006 hardcoded "currency" — leave as-is.

Same rules: build, tests, stop on mismatch, do not commit.
