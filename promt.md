SELECT r.[Review_id], r.[Customer_name]
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
JOIN dbo.[02_CORE_01_Samples] s WITH (NOLOCK) ON s.[Sample_id] = r.[Sample_id]
WHERE r.[Sample_id] = 354
  AND s.[Closed] = 0
  AND r.[Start_date] IS NOT NULL
  AND r.[Completed_date] IS NULL
  AND r.[Cancelled] = 0
  AND r.[Review_id] = 20120;


  -- QA pe chalao, saare 6 buckets ka count ek saath
-- (ye backend logic mirror karta hai)
SELECT 'Finalized' AS Bucket, COUNT(*) AS Cnt
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Review_finalized_date] IS NOT NULL
UNION ALL
SELECT 'Distributed', COUNT(*)
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Review_distributed_date] IS NOT NULL AND r.[Review_finalized_date] IS NULL
UNION ALL
SELECT 'Approved', COUNT(*)
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Review_approval_date] IS NOT NULL AND r.[Review_distributed_date] IS NULL AND r.[Review_finalized_date] IS NULL
UNION ALL
SELECT 'Draft Completed', COUNT(*)
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Completed_date] IS NOT NULL AND r.[Review_approval_date] IS NULL AND r.[Review_distributed_date] IS NULL AND r.[Review_finalized_date] IS NULL AND (r.[Cancelled] IS NULL OR r.[Cancelled]=0)
UNION ALL
SELECT 'In Progress', COUNT(*)
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Start_date] IS NOT NULL AND r.[Completed_date] IS NULL AND r.[Cancelled]=0 AND r.[Review_approval_date] IS NULL AND r.[Review_distributed_date] IS NULL AND r.[Review_finalized_date] IS NULL
UNION ALL
SELECT 'Unopened/Cancelled', COUNT(*)
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND (r.[Start_date] IS NULL OR r.[Cancelled]=1) AND r.[Completed_date] IS NULL AND r.[Review_approval_date] IS NULL AND r.[Review_distributed_date] IS NULL AND r.[Review_finalized_date] IS NULL;

-- Review Queue ki status-derivation (GetQueueRowsAsync jaisa)
SELECT [Review_id], [Customer_name],
  CASE
    WHEN [Cancelled] = 1 THEN 'Cancelled'
    WHEN [Review_approval_date] IS NOT NULL THEN 'Approved'
    WHEN [Review_finalized_date] IS NOT NULL THEN 'Finalized'
    WHEN [Completed_date] IS NOT NULL THEN 'Draft Completed'
    WHEN [Review_distributed_date] IS NOT NULL THEN 'Draft Distributed'
    WHEN [Start_date] IS NOT NULL THEN 'Open'
    ELSE 'Not Open'
  END AS QueueStatus
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 20120;
