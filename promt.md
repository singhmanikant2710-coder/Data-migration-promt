Bug 220 update — Geoff confirmed (via Teams) that "Waived" covenants MUST be included as Non-Compliant, alongside the already-working "Not Compliant" and "Past Due". SINGLE FILE. Show diff, do NOT commit.

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlNonCompliantCovenantsReportRepository.cs

The normalized status predicate (~line 360-365) currently matches:
  REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status], '')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')

Add 'WAIVED' to that IN list (normalized form, no hyphen/space stripping needed since "Waived" has neither):
  IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE','WAIVED')

Do NOT change the Covenant_financial_result check (that one already handles a separate field — confirm whether "Waived" applies there too, or only to Covenant_last_eval_status; check the data pattern from our earlier query results, where Waived appeared only in Covenant_last_eval_status, not Covenant_financial_result).

Do NOT touch IsPerformanceCategory, the routing, the PDF component, or anything else already working. Only this one predicate addition.

Show diff. Rebuild. Do NOT commit.
