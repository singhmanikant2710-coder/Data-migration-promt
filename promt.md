Backend only. Single file: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
Use LIVE DB, ignore columns.csv. Do NOT modify or revert any other logic (including Jothi's). Do NOT change any SELECT list, JOIN, ORDER BY, or statusCounts. Do not plan. Just apply.

UAT #163 (follow-up to #157): Some reviews jumped workflow stages, ending up with Start_date set, Completed_date NULL, but populated approval/distributed/finalized dates. The In Progress bucket only checks Start_date IS NOT NULL AND Completed_date IS NULL, so it wrongly ALSO catches these finalized/distributed reviews, making them appear in both In Progress and their true bucket and throwing off the counts versus Review Queue.

In GetInProgressAsync ONLY, ADD three exclusions to its WHERE clause so In Progress sits at the bottom of the precedence (Finalized > Distributed > Approved > Draft Completed > In Progress):
     AND r.[Review_approval_date] IS NULL
     AND r.[Review_distributed_date] IS NULL
     AND r.[Review_finalized_date] IS NULL

Keep every existing predicate exactly as-is (Start_date IS NOT NULL, Completed_date IS NULL, Cancelled = 0, the @sampleId filter, s.[Closed]=0, and the date-range filter). Change ONLY GetInProgressAsync. Do not touch the other five bucket methods.
