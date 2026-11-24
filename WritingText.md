SELECT COUNT(*) AS Expected_reviews
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE (r.Cancelled IS NULL OR r.Cancelled = 0)
  AND r.Review_finalized_date IS NOT NULL
  AND r.Segment IS NOT NULL
  AND r.Completed_date IS NOT NULL
  AND LTRIM(RTRIM(r.Unit)) = 'Central';
