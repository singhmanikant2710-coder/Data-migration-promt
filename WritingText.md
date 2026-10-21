SELECT TOP 5 r.Review_id, r.Sample_id, s.Closed, r.Locked,
       r.Review_approval_date, r.Review_distributed_date, r.Review_finalized_date
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE ISNULL(s.Closed, 0) = 0
  AND r.Review_approval_date IS NOT NULL
  AND r.Review_finalized_date IS NOT NULL;
