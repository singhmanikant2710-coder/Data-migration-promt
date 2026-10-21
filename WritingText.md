SELECT r.Review_id, r.Locked, r.Review_approval_date,
       r.Review_distributed_date, r.Review_finalized_date
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.Review_id = 21864;
