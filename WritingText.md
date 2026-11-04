SELECT c.name AS ColumnName, t.name AS TypeName
FROM sys.columns c
JOIN sys.types t ON t.user_type_id = c.user_type_id
WHERE c.object_id = OBJECT_ID('dbo.[02_CORE_02_Reviews]')
  AND t.name IN ('timestamp', 'rowversion');

#232: The DBA script failed in Dev – dbo.[02_CORE_02_Reviews] already has a rowversion/timestamp column: [<COL>]. SQL Server allows only one. Nothing was changed in the DB.

Change #232 to use the EXISTING column [<COL>] instead of adding Row_version:
1. Replace every reference to Row_version (repository version read, TryClaimReviewVersionAsync, TouchReviewVersionAsync, deployment guard COL_LENGTH check, load queries) with [<COL>].
2. Core Export: restore it to exactly how it was before #232 (the existing column was already part of the table, so the CSV must stay as it was before). Report what the export did with this column before.
3. Search the repo for any existing use of [<COL>] (EF mapping, legacy Access notes, scripts) and report whether our code changes its behaviour.
4. scripts/sql/add-reviews-row-version.sql: replace it with a read-only check script that confirms [<COL>] exists and is rowversion/timestamp (for the DBA to run in QA/Prod), or delete it if not needed – recommend which.
5. Deployment note: since the column already exists, protection turns on as soon as the code is deployed. Confirm the guard still works and report anything that changes because of this.

Report in chat: ADDED/CHANGED/REMOVED (file:line), dotnet build, npx tsc --noEmit, npm run build, git status. Do not commit or push.