SELECT r.Review_id, r.Customer_name, r.Sample_id, r.Segment,
       r.Completed_date, r.Review_approval_date, r.Review_finalized_date
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE r.Review_finalized_date IS NOT NULL
  AND (r.Cancelled IS NULL OR r.Cancelled = 0)
  AND (r.Completed_date IS NULL OR r.Segment IS NULL)
ORDER BY r.Review_id;
