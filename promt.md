Formula validated — Variant B confirmed against all customers in the database
(zero formula-attributable mismatches; all remaining mismatches classified as
pre-existing corruption from the active defect), AND independently confirmed
against legacy MS Access for two different fiscal-start-months (start=1:
Charter Pipe LLC 201801 → FY2018/FM1 matches; start=6: Imperial Trading Co
LLC 201806-201808 → FY2019/FM1,2,3 all match). Also confirmed empty on the
tblCustomer-vs-tblMain consistency check, so tblCustomer is a reliable source.

Use this formula:
fy = (start = 1) ? y : ((m >= start) ? y + 1 : y)
fm = ((m - start + 12) % 12) + 1

Additional confirmed evidence to fold into scoping (not new defects, just
sharper evidence for the ones already diagnosed):
- HCG Consumer Credit II Trust has rows with intFiscalYear = 0 (months
  202506, 202507) — a value no formula could produce. This confirms a
  partial write survives even when the save should fail/rollback,
  consistent with the PK-collision defect already targeted.
- Bankers Healthcare Group LLC shows a fresh corruption onset at 202605-
  202606, separate from and more recent than its earlier corrupted run —
  confirms the defect is still actively corrupting data now, not just
  historically. This raises the priority of shipping the fix but does not
  change its scope.

Proceed with the diff as previously scoped: UpsertRowWithConnectionAsync
(probe and UPDATE narrowed to strCustomerName + strMonthKey only, primary
key never rewritten, prevRow+1 fiscal derivation replaced with the
validated formula above) and MainController (covenant seed moved before
the write).

Constraints (unchanged):
- Show the diff only, do not apply it.
- List every distinct intFiscalYearMonthStart value present in tblCustomer
  today, so I can pick one test customer per value for regression testing.
- For a calendar-FY customer (start = 1), confirm in writing the new
  formula produces byte-identical (fy, fm) to what the old calendar-only
  DeriveFiscalFromMonthKey produced for start=1 — that path must not
  change at all.
- Re-list every other caller of UpsertRowWithConnectionAsync,
  ApplyIncomingCovenantsToSlotsAsync, and DeriveFiscalFromMonthKey.
- Do not modify AccessMainRepository.cs — flag only.
- Do not touch MapMetricPoint, TryMergeCovenantsIntoSeries, covNumeric, or
  any threshold-vs-actual display logic.
- Do NOT write a data-repair/backfill script yet — that is explicitly a
  separate task, scoped only after this write-path fix is approved and
  applied. This prompt is for the go-forward fix only.
