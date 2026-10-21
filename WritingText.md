SELECT TOP 5 r.Review_id, s.Sample_name, r.Review_approved_date,
       r.Review_distributed_date, r.Review_finalized_date, r.Locked
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE s.Closed = 1
  AND r.Review_finalized_date IS NOT NULL
  AND r.Review_approved_date IS NOT NULL
  AND (r.Cancelled IS NULL OR r.Cancelled = 0);
