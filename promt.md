-- QA DB pe chalao. Pehle 20120 ka data dekho
SELECT [Review_id], [Sample_id], [Cancelled], [Start_date], [Completed_date],
       [Review_approval_date], [Review_distributed_date], [Review_finalized_date]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 20120;
