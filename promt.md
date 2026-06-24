SELECT
  -- borrowersSampled should equal sum of all buckets below
  COUNT(*) AS total_non_cancelled,

  SUM(CASE WHEN Review_approval_date IS NOT NULL THEN 1 ELSE 0 END) AS approved,

  SUM(CASE WHEN Review_finalized_date IS NOT NULL 
            AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS finalized,

  SUM(CASE WHEN Completed_date IS NOT NULL 
            AND Review_finalized_date IS NULL 
            AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS draft_completed,

  SUM(CASE WHEN Review_distributed_date IS NOT NULL 
            AND Completed_date IS NULL 
            AND Review_finalized_date IS NULL 
            AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS distributed,

  SUM(CASE WHEN Start_date IS NOT NULL 
            AND Review_distributed_date IS NULL 
            AND Completed_date IS NULL 
            AND Review_finalized_date IS NULL 
            AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS in_progress,

  SUM(CASE WHEN Cancelled = 1 THEN 1 ELSE 0 END) AS cancelled_count
FROM dbo.[02_CORE_02_Reviews]
WHERE Cancelled IS NULL OR Cancelled = 0;
