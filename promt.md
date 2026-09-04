SELECT
    r.Customer_number, r.Sample_id, r.Sample_name,
    r.Sample_date, r.Sample_finalized_date, r.Review_approval_date, r.Cancelled,
    s.Sample_id AS joined_sample_id,
    CASE WHEN r.Sample_date >= DATEADD(month,-6,GETDATE()) THEN 'HELD_at_6' ELSE 'pass_at_6' END AS at6,
    CASE WHEN r.Sample_date >= DATEADD(month,-9,GETDATE()) THEN 'HELD_at_9' ELSE 'pass_at_9' END AS at9
FROM dbo.[02_CORE_02_Reviews] r
LEFT JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE LTRIM(RTRIM(r.Customer_number)) IN ('84538953','84551519','84554592')
ORDER BY r.Customer_number, r.Review_finalized_date DESC;
