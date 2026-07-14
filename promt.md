SELECT
  COUNT(*)                                                                    AS BorrowersSampled,
  SUM(CASE WHEN [Start_date] IS NULL OR [Cancelled] = 1 THEN 1 ELSE 0 END)   AS UnopenedCancelled,
  SUM(CASE WHEN [Start_date] IS NOT NULL THEN 1 ELSE 0 END)                  AS InProgress,
  SUM(CASE WHEN [Completed_date] IS NOT NULL THEN 1 ELSE 0 END)              AS DraftCompleted,
  SUM(CASE WHEN [Review_approval_date] IS NOT NULL THEN 1 ELSE 0 END)        AS Approved,
  SUM(CASE WHEN [Review_distributed_date] IS NOT NULL THEN 1 ELSE 0 END)     AS Distributed,
  SUM(CASE WHEN [Review_finalized_date] IS NOT NULL THEN 1 ELSE 0 END)       AS Finalized
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK);
