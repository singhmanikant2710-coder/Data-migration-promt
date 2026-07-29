SELECT COUNT(*) FROM dbo.[02_CORE_02_Reviews] r JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0 AND r.[Review_finalized_date] IS NOT NULL AND r.[Review_id]=20120;
