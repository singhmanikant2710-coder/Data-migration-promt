Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs

This file has 6 methods that each return r.[TTBA_exposure] for Exposure (often 0):
GetCompletedDraftsAsync, GetInProgressAsync, GetDistributedAsync, 
GetFinalizedAsync, GetApprovedAsync, GetUnopenedOrCancelledAsync.

For ALL SIX methods, change exposure to be computed the SAME way as the Review Queue 
(GetQueueRowsAsync) — summing Commitment from Accounts by Review_id.

In EACH of the 6 SQL queries:

1. Add this LEFT JOIN right after "FROM dbo.[02_CORE_02_Reviews] AS r WITH (NOLOCK)":

    LEFT JOIN (
        SELECT a.[Review_id], SUM(COALESCE(a.[Commitment], 0)) AS [CommittedExposure]
        FROM dbo.[02_CORE_04_Accounts] AS a WITH (NOLOCK)
        GROUP BY a.[Review_id]
    ) AS agg ON agg.[Review_id] = r.[Review_id]

2. In each SELECT list, ADD: agg.[CommittedExposure] AS [AccountsCommittedExposure]
   (keep r.[TTBA_exposure] as-is).

3. Update the C# reader/mapping for each method so the Exposure field is populated 
   from [AccountsCommittedExposure] instead of [TTBA_exposure] (treat null as 0). 
   Apply to all 6.

Match the exact JOIN/column style used in GetQueueRowsAsync and the same change just 
made in SqlReviewHistoryRepository.cs.

Modify ONLY SqlReviewStatusRepository.cs. If the row model/mapping is in another file 
and must change, STOP and tell me first.
