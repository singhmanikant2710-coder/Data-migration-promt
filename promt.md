-- Cancellation rationale column dhundo
SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND (COLUMN_NAME LIKE '%cancel%' OR COLUMN_NAME LIKE '%rational%');

  -- Ek cancelled review dekho
SELECT TOP 5 [Review_id], [Sample_id], [Customer_name], [Cancelled],
       [Start_date], [Completed_date]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Cancelled] = 1;

Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #159): Cancelled reviews open in the Review Form with all NULL values. The client needs to be able to locate cancelled reviews from Review Queue and Review Status, open them, and see the Cancellation Rationale and other populated fields.

Report:
1) In backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs — the three header query methods (GetReviewHeaderByIdAsync, GetLatestReviewHeaderForSampleAndEcifAsync, GetLatestReviewHeaderForEcifAsync). Paste each WHERE clause. Do they exclude cancelled reviews (e.g. AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0))? This would explain the all-NULL form.
2) In the same file, GetQueueRowsAsync — paste its WHERE clause. Does it exclude cancelled reviews from the Review Queue grid?
3) In SqlReviewStatusRepository.cs — does the Unopened/Cancelled bucket return cancelled reviews, and are they clickable through to the Review Form? Paste the relevant predicate.
4) Is there a Cancellation Rationale field anywhere in the codebase — in the ReviewInfoSection contract, the frontend types, or rendered on the Review Form? Search for "cancel", "rationale", "cancellationRationale". Paste anything found, or state that none exists.
5) State exactly what must change and in how many files so that: (a) cancelled reviews are reachable from Review Queue and Review Status, (b) opening a cancelled review loads its populated data rather than NULLs, and (c) the Cancellation Rationale is displayed.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.


