Before I accept, confirm these 4 in the diffs (paste the relevant lines):
1. reports/page.tsx routing: show the reordered/tightened logic — "Non-Compliant Covenants" now maps to non-compliant-covenants id, "Covenants Summary" still maps correctly. Confirm the dead-branch bug is fixed.
2. SqlNonCompliantCovenantsReportRepository.cs: show the covenant status predicate — confirm it includes 'Not Compliant' AND 'Past Due' (widened from NON-COMPLIANT only).
3. Same repo: confirm exposure = SUM(Commitment), NOT Balance. Paste the aggregation.
4. NonCompliantCovenantsPDF.tsx: confirm the details table has exactly these 8 columns in order (CUSTOMER NAME (REVIEW ID) | STATUS | CATEGORY | COVENANT TYPE | THRESHOLD | EVAL DATE | RESULT | COMMITMENT), and THRESHOLD/RESULT render blank for Monitoring covenants.
Also confirm: SqlCrmSummaryTableReportRepository's caption output is unchanged (the shared SqlReportCaptionHelper produces the same string as before) — that report must not regress.
