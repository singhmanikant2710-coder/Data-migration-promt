Two things before I approve anything:

1. Bug: SqlCustomerRepository's new prior-fiscal-start read casts
   ExecuteScalarAsync's result directly to (long?) — intFiscalYearMonthStart
   is smallint, so this will throw InvalidCastException at runtime. Use the
   existing ToInt() helper (already used elsewhere in this class) instead
   of the raw cast. Fix this and resubmit just that hunk.

2. Proceeding with: include both extra helpers (item 1 = yes), transaction
   option 1 + batched Phase A (option 3), idempotent cascade + separate
   admin recompute endpoint (item 3b), ship the frontend cache-invalidator
   now (item 4 = yes), accept the interface signature change (item 5 = yes).

Regenerate the diff with the bug fixed and the batched Phase A. Show diff
only, don't apply.
