SELECT COUNT(*) AS total_finalized,
       SUM(CASE WHEN Sample_name IS NULL OR Sample_name = '' THEN 1 ELSE 0 END) AS null_or_empty_name
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_finalized_date IS NOT NULL
  AND (Cancelled IS NULL OR Cancelled = 0);
