Fix Delinquent_status import corruption at the source. Two changes, both in the Monthly Upload pipeline. Show diffs, do NOT commit.

FILE 1: frontend/src/app/api/monthly-upload/parse/route.ts and save/route.ts
Add validation for the DelinquentID column: the valid domain is a small known set (curr, 1-30, 30-59, 60-89, 90+, and their "NonAccr " prefixed variants). If an uploaded value for this column does NOT match that pattern (case-insensitive), OR matches a date-shaped pattern (e.g. looks like it was Excel-coerced — day-month text patterns), flag it as an import error/warning for that row rather than silently accepting it. Follow the existing error/warning reporting pattern already used elsewhere in this upload flow (however parse errors are currently surfaced to the user).

FILE 2: Fix the dead/broken .xlsx template path reference (parse/route.ts:104, save/route.ts:150) — both point to "01_DATA_01_Data Mart Trial_LOAD.xlsx" which doesn't exist (only the .csv does). Either remove this dead code path if headers are genuinely resolved from schema.ts (confirm first), or fix the reference if it's actually needed.

Do NOT change the account-load SQL (SqlSampleLoadRepository.cs) — confirmed clean, not the source of the issue.

Show diffs. Rebuild. Do NOT commit.
