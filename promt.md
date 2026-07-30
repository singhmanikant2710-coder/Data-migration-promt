-- WITHOUT date range (Queue-style) — expect 15
SELECT COUNT(*) FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE r.Sample_id = @sample354Id AND s.Closed = 0
  AND (r.Cancelled IS NULL OR r.Cancelled = 0)
  AND r.Start_date IS NOT NULL AND r.Completed_date IS NULL
  AND r.Review_approval_date IS NULL AND r.Review_distributed_date IS NULL
  AND r.Review_finalized_date IS NULL;

-- WITH date range (Status-style) — expect 14
SELECT COUNT(*) FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE r.Sample_id = @sample354Id AND s.Closed = 0
  AND (r.Cancelled IS NULL OR r.Cancelled = 0)
  AND r.Start_date IS NOT NULL AND r.Completed_date IS NULL
  AND r.Review_approval_date IS NULL AND r.Review_distributed_date IS NULL
  AND r.Review_finalized_date IS NULL
  AND r.Start_date >= @startDate
  AND r.Start_date < DATEADD(day, 1, @endDate);
