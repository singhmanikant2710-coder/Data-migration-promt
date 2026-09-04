SELECT r.Customer_number, r.Sample_name, r.Sample_date,
  CASE WHEN r.Sample_date >= DATEADD(month,-9,CONVERT(date,GETDATE())) THEN 'HOLD' ELSE 'pass' END AS new_logic
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.Sample_id = 354 AND (r.Cancelled IS NULL OR r.Cancelled=0);
