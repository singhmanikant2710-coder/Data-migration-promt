SELECT [Review_id], [Sample_id], [Customer_name], [Cancelled],
       [Start_date], [Completed_date], [Review_approval_date],
       [Review_distributed_date], [Review_finalized_date]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 20120;

SELECT s.[Sample_id], s.[Sample_name], s.[Closed]
FROM dbo.[02_CORE_01_Samples] s WITH (NOLOCK)
WHERE s.[Sample_id] = 354;

SELECT r.[Review_id], r.[Customer_name]
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
JOIN dbo.[02_CORE_01_Samples] s WITH (NOLOCK) ON s.[Sample_id] = r.[Sample_id]
WHERE (@sampleId IS NULL OR r.[Sample_id] = 354)
  AND s.[Closed] = 0
  AND r.[Review_finalized_date] IS NOT NULL
  AND r.[Review_id] = 20120;
