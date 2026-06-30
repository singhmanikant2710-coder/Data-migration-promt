Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs

In GetHistoryRowsAsync, the SQL currently returns r.[TTBA_exposure] for Exposure, 
which is often 0. Change it to compute exposure the SAME way as the Review Queue 
(GetQueueRowsAsync) does — summing Commitment from the Accounts table by Review_id.

Make these changes to the SQL:

1. Add this LEFT JOIN right after "FROM dbo.[02_CORE_02_Reviews] AS r WITH (NOLOCK)":

    LEFT JOIN (
        SELECT a.[Review_id], SUM(COALESCE(a.[Commitment], 0)) AS [CommittedExposure]
        FROM dbo.[02_CORE_04_Accounts] AS a WITH (NOLOCK)
        GROUP BY a.[Review_id]
    ) AS agg ON agg.[Review_id] = r.[Review_id]

2. In the SELECT list, ADD this column (keep r.[TTBA_exposure] as-is, just add the new one):

    agg.[CommittedExposure] AS [AccountsCommittedExposure]

3. Then update the C# mapping/reader code that builds the history row so that the 
   Exposure field is populated from [AccountsCommittedExposure] instead of 
   [TTBA_exposure]. Show me where the reader maps the exposure column and switch it 
   to read AccountsCommittedExposure (treat null as 0).

Match the exact JOIN/column style used in GetQueueRowsAsync in SqlReviewRepository.cs.

Modify ONLY SqlReviewHistoryRepository.cs. If the C# row model or mapping lives in 
another file and must change, STOP and tell me first.
