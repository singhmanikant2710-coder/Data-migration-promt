SELECT
  SUM(CASE WHEN Review_finalized_date IS NOT NULL AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS finalized_bucket,
  SUM(CASE WHEN Review_finalized_date IS NOT NULL THEN 1 ELSE 0 END) AS any_finalized,
  SUM(CASE WHEN Review_distributed_date IS NOT NULL AND Completed_date IS NULL AND Review_finalized_date IS NULL AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS distributed_bucket,
  SUM(CASE WHEN Review_distributed_date IS NOT NULL THEN 1 ELSE 0 END) AS any_distributed,
  SUM(CASE WHEN Review_approval_date IS NOT NULL THEN 1 ELSE 0 END) AS any_approved
FROM dbo.[02_CORE_02_Reviews]
WHERE Cancelled IS NULL OR Cancelled = 0;
