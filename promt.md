SELECT r.[Review_id], r.[Customer_name]
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 
  AND r.[Review_finalized_date] IS NOT NULL 
  AND r.[Review_id]=20120;

  SELECT r.[Review_id], r.[Customer_name]
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 
  AND r.[Review_distributed_date] IS NOT NULL 
  AND r.[Review_finalized_date] IS NULL
  AND r.[Review_id]=20120;

  SELECT r.[Review_id], r.[Customer_name]
FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 
  AND r.[Start_date] IS NOT NULL AND r.[Completed_date] IS NULL AND r.[Cancelled]=0
  AND r.[Review_approval_date] IS NULL AND r.[Review_distributed_date] IS NULL AND r.[Review_finalized_date] IS NULL
  AND r.[Review_id]=20120;
