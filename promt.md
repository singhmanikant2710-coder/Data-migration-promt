SELECT [Review_id], [Sample_id], [Customer_name], [Cancelled],
       [Start_date], [Completed_date], [Review_approval_date],
       [Review_distributed_date], [Review_finalized_date]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 20120;

SELECT s.[Sample_id], s.[Sample_name], s.[Closed]
FROM dbo.[02_CORE_01_Samples] s WITH (NOLOCK)
WHERE s.[Sample_id] = 354;
