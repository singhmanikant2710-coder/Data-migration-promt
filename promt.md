Task: Fix UAT Bug #121 only. Make the smallest safe change required.

IMPORTANT SAFETY RULES:
- Do NOT modify, refactor, reformat, rename, or clean up unrelated existing code.
- Do NOT change anyone else’s work, including Jothi’s changes.
- Do NOT change existing business logic outside the exact scope of Bug #121.
- Do NOT modify backend code, SQL queries, DTOs, APIs, or database objects unless absolutely required. Based on the investigation, backend changes should NOT be required.
- Preserve the behavior of all other Review Status buckets/tabs, including In Progress, Draft Completed, Approved, Distributed, and Finalized.
- Before editing, inspect the current/latest file contents and preserve any existing uncommitted or recently added changes.
- Make the minimum possible code change.
- Do not execute any destructive commands.

BUG #121 CONTEXT:
Screen/Tab: Review Status
Section/Field: Statuses

For rows belonging to the Unopened/Cancelled bucket, the actual "Review Status" column must display the per-row status:

1. "Unopened"
   when:
   [02_CORE_02_Reviews].[Start_date] IS NULL
   AND [02_CORE_02_Reviews].[Cancelled] = 0 / No / False

2. "Cancelled"
   when:
   [02_CORE_02_Reviews].[Cancelled] = 1 / Yes / True

CURRENT INVESTIGATION FINDINGS:

Frontend:
File:
frontend/src/app/review-status/page.tsx

The Review Status grid renders the status using:
{r.status}

However, the Unopened/Cancelled frontend mapping currently hardcodes:
status: "Unopened/Cancelled",

This causes every row in that bucket to display the same generic "Unopened/Cancelled" label.

Backend:
File:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs

Method:
GetUnopenedOrCancelledAsync

The backend already computes the correct per-row value using CASE logic equivalent to:

CASE
    WHEN r.[Cancelled] = 1 THEN 'Cancelled'
    WHEN r.[Start_date] IS NULL THEN 'Unopened'
    ELSE 'Unopened'
END AS [StatusLabel]

The data-reader mapping already reads StatusLabel and assigns it to:
ReviewStatusRow.Status

Therefore, use the existing server-provided per-row Status value instead of replacing it with the hardcoded frontend value.

REQUIRED FIX:

In:
frontend/src/app/review-status/page.tsx

Locate ONLY the mapping that builds rows for the Unopened/Cancelled bucket.

Replace the hardcoded status assignment:

status: "Unopened/Cancelled",

with the API/backend-provided per-row status value.

Use the actual property available on the returned row based on the existing type/API response.

Expected form, if consistent with the current response shape:

status: String(r.status ?? r.Status ?? ""),

Do NOT blindly use this exact expression if the existing typed response clearly exposes only one correct property. Prefer the existing typed property and project conventions.

EXPECTED RESULT:

For the Unopened/Cancelled bucket:

- Start_date = NULL and Cancelled = 0/False
  -> Review Status displays "Unopened"

- Cancelled = 1/True
  -> Review Status displays "Cancelled"

The UI must no longer display the generic "Unopened/Cancelled" value in the Review Status column for individual rows.

SCOPE:
Expected files changed: 1 only

frontend/src/app/review-status/page.tsx

Do not change other bucket mappings.

VALIDATION BEFORE COMPLETING:

1. Confirm the backend-provided status property name from the actual frontend API response/type.
2. Confirm the Review Status cell still renders r.status.
3. Confirm only the Unopened/Cancelled row mapping was changed.
4. Confirm In Progress, Draft Completed, Approved, Distributed, Finalized and all other statuses remain unchanged.
5. Check TypeScript/build/lint errors relevant to the modified code.
6. Show the exact git diff for the change.
7. Report all files modified.
8. If more than the expected 1 file was modified, STOP and explain why before making additional changes.
9. Do not commit or push anything.

Implement Bug #121 now with the minimum safe change.
