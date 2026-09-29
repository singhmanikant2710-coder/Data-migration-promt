FIX (backend, all industries). Remove the call to
EnsureYtdFromMonthlyForRowAsync in UpsertRowWithConnectionAsync
(SqlMainRepository.cs ~1281). Legacy has no such pass;
RecomputeRevenueAndGrossProfitYtdAsync (~1274) already writes the
legacy value (Sum of monthly per fiscal year). Keep the method itself
(unused) — do not delete code beyond the call.

EVIDENCE & SAFETY:
- Quote legacy qryMainYTDCalculations_002 / _005 as evidence.
- Confirm no other caller depends on it.
- Regression: save a month for ATHENS and one ConsumerFinance customer;
  curRevenueOrSalesYTD / curGrossProfitYTD = running sum of monthly
  (use your query A/C to verify). A/R Turn Days and Inventory Turn
  unchanged where YTD was already correct.
- Do not change other recomputes, calculations, or data.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL and zero case
- NOT TOUCHED
