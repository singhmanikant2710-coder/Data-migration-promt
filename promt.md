Backend only. Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only remove the specific WHERE lines named below and ADD new columns. Do not plan. Just apply.

UAT #159: Cancelled reviews open in the Review Form with all NULL values, because the three header queries exclude cancelled rows. They must load normally, and the Cancellation Rationale must be returned.

DB confirmed on dbo.[02_CORE_02_Reviews]: [Cancelled] bit, [Cancelled_date] datetime2, [Cancelled_reason] nvarchar.

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

1) In ALL THREE header query methods — GetReviewHeaderByIdAsync, GetLatestReviewHeaderForSampleAndEcifAsync, GetLatestReviewHeaderForEcifAsync — REMOVE only this cancellation exclusion from the WHERE clause:
     (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
   Keep every other WHERE condition, the joins, ORDER BY, and the SELECT column order exactly as they are. If removing that line leaves a dangling AND, fix the syntax without changing any other condition.

2) In the private ReviewHeader record/class, ADD three properties:
     public bool? Cancelled { get; init; }
     public DateTime? CancelledDate { get; init; }
     public string? CancelledReason { get; init; }

3) In ALL THREE header queries, APPEND these three columns to the END of the SELECT list (do not reorder existing columns):
     r.[Cancelled],
     r.[Cancelled_date],
     r.[Cancelled_reason]
   Read them DBNull-safe using NAME-BASED ordinal lookups (not hardcoded indexes), following the same pattern already used for EIC_Name:
     var ordCancelled       = rdr.GetOrdinal("Cancelled");
     var ordCancelledDate   = rdr.GetOrdinal("Cancelled_date");
     var ordCancelledReason = rdr.GetOrdinal("Cancelled_reason");

4) In BOTH GetReviewByEcifAsync and GetReviewByKeysAsync, where ReviewInfoSection is constructed, ADD:
     Cancelled = row?.Cancelled,
     CancelledDate = row?.CancelledDate,
     CancellationRationale = row?.CancelledReason,
   And ADD the matching properties to the ReviewInfoSection contract (the file where ReviewerName / ExaminerInCharge are declared), placed after the existing fields:
     public bool? Cancelled { get; init; }
     public DateTime? CancelledDate { get; init; }
     public string? CancellationRationale { get; init; }

Do not touch the frontend in this step. Do not change GetQueueRowsAsync or SqlReviewStatusRepository.cs. Report the files changed and the exact removed WHERE lines and new SELECT columns.
