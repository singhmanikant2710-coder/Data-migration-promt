Task: Review Status screen — backend only. Do NOT touch frontend. Do not refactor, do not plan, just apply the minimal changes.

Rules:
- Use the LIVE SQL Server database as the schema source of truth. IGNORE columns.csv (it is stale).
- Read-only diagnostics first: locate the existing Review Status endpoint and repository method that returns the sample dropdown, the date fields, and the status counts. Report the file paths and current SQL before editing.
- Single file per edit. Do not rename existing DTO properties that the frontend already consumes — only ADD new properties. Existing response contract must remain backward compatible.

Required changes:

1) Sample dropdown source (UAT #77)
   - The "Select Sample / Review Name" list must return ONLY open samples:
     dbo.[02_CORE_01_Samples] WHERE [Closed] = 0 (No/False).
   - Do not change the shape of the returned items, only add the WHERE filter.

2) Sample date fields (UAT #78)
   - Return [Sample_start_date] and [Sample_end_date] from dbo.[02_CORE_01_Samples] for the SELECTED sample.
   - These must be nullable in the DTO and returned as NULL when no sample is selected.
   - Add them to the existing Review Status response DTO as new nullable DateTime? properties (e.g. SampleStartDate, SampleEndDate). Do not remove anything.

3) Status bucket logic (UAT #79)
   Compute the counts from dbo.[02_CORE_02_Reviews] using EXACTLY this logic. Each review may satisfy multiple conditions — the existing count semantics used by the grid/bucket filter must be preserved; only make sure these six buckets are all present and derived from these columns:
   - Unopened/Cancelled : [Start_date] IS NULL OR [Cancelled] = 1
   - In Progress        : [Start_date] IS NOT NULL
   - Draft Completed    : [Completed_date] IS NOT NULL
   - Approved           : [Review_approval_date] IS NOT NULL
   - Draft Distributed  : [Review_distributed_date] IS NOT NULL
   - Finalized          : [Review_finalized_date] IS NOT NULL
   - Borrowers Sampled  : total row count (unchanged)
   Before editing, VERIFY every one of these column names exists on the live dbo.[02_CORE_02_Reviews]. If any column is missing, STOP and report it instead of guessing.

Do not change any other screen, repository, or shared service. After the edit, run read-only diagnostics on the changed file only.


---‐----------------------------------×××××××××××@@@@@

Task: Frontend only — file: frontend/src/app/review-status/page.tsx (locate the actual Review Status page component first and confirm the path before editing). Single file. Do not read unrelated files, do not plan, just apply.

Constraint: Do NOT change any existing filtering, search, pagination, page-size, or count-derivation logic. The grid and the totals must keep working exactly as they do today. Only labels, ordering, colors, editability, and button removal.

Changes:

1) (UAT #77) Rename the label "Select Sample / Review Name" to "Select Sample Name".
   Increase the dropdown's font size so it visually matches the Sample Start Date / Sample End Date input fields to its right (same text size/line-height as those date inputs).

2) (UAT #78) Sample Start Date and Sample End Date inputs must be READ-ONLY (disabled / non-editable, no date picker interaction). Bind them to sampleStartDate / sampleEndDate returned by the API. When no sample is selected, render them EMPTY (null), not today's date and not a placeholder date.

3) (UAT #79) Reorder the status squares left-to-right to exactly:
   Borrowers Sampled, Unopened/Cancelled, In Progress, Draft Completed, Approved, Distributed, Finalized.
   Apply this color schema:
   - Borrowers Sampled  : dark navy   (#1B3A5C style — reuse the existing dark navy header token)
   - Unopened/Cancelled : medium blue (#2E86D6)
   - In Progress        : medium blue (#2E86D6)
   - Draft Completed    : medium blue (#2E86D6)
   - Approved           : medium blue (#2E86D6)
   - Distributed        : medium blue (#2E86D6)
   - Finalized          : green       (#1FA84C)
   Keep the existing click-to-filter behaviour on each square, and keep the numbers bound to the same count values as before — only reorder and recolor.

4) (UAT #80) Reorder the "Bucket" dropdown options to exactly:
   Unopened / Cancelled, In Progress, Draft Completed, Approved, Draft Distributed, Finalized, All Statuses.
   Do NOT change the underlying option VALUES that the filter logic keys off — only the display order (and the "Distributed" label to "Draft Distributed" if the value stays the same).

5) (UAT #81) Remove the "Refresh" and "Close" buttons from the Review Status header. Keep the "Home" button. Delete any handler that becomes unused; do not delete shared utilities.

After the edit: run read-only TypeScript diagnostics on this file only and report errors.
