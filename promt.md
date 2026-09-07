Bug 220 — the Non-Compliant Covenants predicate misses real data. The actual DB status values are: 'Not-Compliant' (hyphen, no space), 'Past Due', 'Waived', 'Compliant', 'Not Due'. Our predicate uses 'NON-COMPLIANT' and 'NOT COMPLIANT' (space) — neither matches the actual 'NOT-COMPLIANT' (hyphen). So Not-Compliant rows are dropped and the report looks empty.

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlNonCompliantCovenantsReportRepository.cs

Fix the status predicate (both the eval_status and financial_result checks) to match the ACTUAL stored values. Make it robust to hyphen/space variants by normalizing before comparison. Change the predicate to strip hyphens/spaces OR list all real variants. Safest approach — normalize by removing spaces and hyphens, then compare:

  REPLACE(REPLACE(UPPER(LTRIM(RTRIM(ISNULL(c.[Covenant_last_eval_status],'')))), '-', ''), ' ', '') IN ('NONCOMPLIANT','NOTCOMPLIANT','PASTDUE')

Apply the same normalization to the Covenant_financial_result check. This way 'Not-Compliant', 'NON-COMPLIANT', 'Not Compliant', 'Past Due', 'PAST-DUE' all match correctly regardless of hyphen/space. Keep 'Waived' EXCLUDED (per Geoff). 

Confirm: 'Not-Compliant' and 'Past Due' now match; 'Waived', 'Compliant', 'Not Due', 'Not Due', and junk values (DONE/Testing) do NOT match.

Show the diff. Rebuild. Do NOT commit.
