#232 – switch from the new Row_version column to the EXISTING timestamp column

FACT (verified in Dev SSMS)
- The DBA script failed: dbo.[02_CORE_02_Reviews] already has a timestamp column named [SSMA_TimeStamp] (type timestamp, NOT NULL, COL_LENGTH = 8). SQL Server allows only one per table, so Row_version can never be added. Nothing was changed in the DB.
- [SSMA_TimeStamp] already changes automatically on every UPDATE, so it works as the review version without any schema change.

CHANGES (touch only what is needed)
1. Replace every Row_version reference in the backend with [SSMA_TimeStamp]: version read (GetReviewRowVersionTokenAsync), TryClaimReviewVersionAsync, TouchReviewVersionAsync, the deployment-guard COL_LENGTH check, the load queries (SqlReviewRepository.cs ~156, ~323, ~2886, ~2924, ~2943-2956) and anything else found by searching the whole repo for "Row_version".
2. ExportsController.cs: revert it to exactly how it was at git HEAD (before #232), so the Core Export CSV is unchanged. Report what the export already did with [SSMA_TimeStamp] before #232 (included, skipped, how it was formatted).
3. Search the whole repo for existing uses of SSMA_TimeStamp (EF mappings, DTOs, SELECT * mapping, scripts, legacy notes). Report each hit with file:line and whether our version logic changes its behaviour.
4. Replace scripts/sql/add-reviews-row-version.sql with a READ-ONLY check script named scripts/sql/check-reviews-ssma-timestamp.sql. It must confirm that [SSMA_TimeStamp] exists on dbo.[02_CORE_02_Reviews] and is timestamp/rowversion, for the DBA to run in QA/Prod before deploy. No ALTER. Delete the old file.
5. Deployment guard: keep it, checking [SSMA_TimeStamp]. Since the column already exists, protection turns on as soon as the code is deployed. Confirm the guard still behaves as before when the column is missing (e.g. in another environment), and report anything else that changes because of this.
Do not change anything else (#212, #233, #235, #239/#240, #249, #254 and the PE delete fix stay untouched).

REPORT (in chat, not in a file)
- ADDED / CHANGED / REMOVED with file:line.
- Confirm a repo-wide search for "Row_version" now returns 0 hits.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave the pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
