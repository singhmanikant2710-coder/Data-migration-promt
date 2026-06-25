SELECT 
  SUM(CASE WHEN Review_finalized_date IS NOT NULL 
            AND Review_approval_date IS NULL THEN 1 ELSE 0 END) AS finalized_not_approved,
  SUM(CASE WHEN Review_finalized_date IS NOT NULL 
            AND Review_approval_date IS NOT NULL 
            AND Review_finalized_date <> Review_approval_date THEN 1 ELSE 0 END) AS finalized_approved_different_dates
FROM dbo.[02_CORE_02_Reviews]
WHERE Cancelled IS NULL OR Cancelled = 0;
