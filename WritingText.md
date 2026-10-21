SELECT r.Review_id, s.Closed, r.Locked,
       r.Review_approved_date, r.Review_distributed_date, r.Review_finalized_date
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE r.Review_id = 21864;
