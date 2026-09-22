Small addition to the approved diff: the UPDATE branch in
UpsertRowWithConnectionAsync never sets datFiscalYearStart (only the
INSERT branch does), so any existing/corrupted row that gets edited has
its intFiscalYear/intFiscalMonth self-corrected on save, but
datFiscalYearStart stays permanently stale.

Confirmed with real data: Nationwide Specialty Finance Inc, monthKey
202601 — after an edit-save, intFiscalYear=2026/intFiscalMonth=8 are
now correct, but datFiscalYearStart is still 2025-06-01 instead of the
expected 2026-06-01.

Add datFiscalYearStart to the UPDATE branch's SET list too, using the
same computation as the INSERT branch (new DateTime(fy, fiscalStartMonth,
1)) — fy and fiscalStartMonth are already resolved earlier in the method,
so this should just be reusing existing values, not new logic. Show diff
only.
