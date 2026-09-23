Design a cascade recompute for when a customer's intFiscalYearMonthStart
changes. Scope, per John's decision (full cascade, option b):

1. In SqlCustomerRepository.UpdateCustomerProfileAsync, detect when
   FiscalStartMonth actually changes (compare against the customer's
   current stored value before the UPDATE — only trigger cascade on a
   real change, not every profile save).

2. On change, after updating tblCustomer, loop through every existing
   tblMain row for that customer (ordered by strMonthKey) and recompute,
   reusing the existing per-row logic already used by the save path
   (the validated DeriveFiscalFromMonthKey formula, the
   datFiscalYearStart computation, UpdateElapsedFiscalDaysForRowsAsync,
   UpdateArTurnDaysForRowAsync, UpdateInventoryTurnForRowAsync) rather
   than duplicating that logic.

3. After per-row recompute, re-run the fiscal-year-scoped YTD/TTM
   aggregate functions (RecomputePbtYtdAsync and siblings) for EVERY
   distinct fiscal year now present for that customer — not just one,
   since re-partitioning can shift which months belong to which year.

4. Invalidate ContextController's context:fiscalStart:{customer} cache
   entry on the same request.

5. Flag (don't yet solve) the client-side session cache in lookups.ts
   getFiscalStartMonth — note whether the frontend needs a matching
   change or if a page reload/re-fetch already handles it.

Report back as a design + diff, not applied. Flag performance concerns
for customers with 100+ months of history (should this run in one
transaction, or does it need batching/async), and flag if wrapping all
of this in one request risks a timeout — propose an approach but don't
assume a specific answer without calling it out for review.
