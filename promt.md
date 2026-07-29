SELECT r.[Review_id], r.[Review_finalized_date], s.[Sample_start_date], s.[Sample_end_date]
FROM dbo.[02_CORE_02_Reviews] r 
JOIN dbo.[02_CORE_01_Samples] s ON s.[Sample_id]=r.[Sample_id]
WHERE r.[Sample_id]=354 AND s.[Closed]=0
  AND r.[Review_finalized_date] IS NOT NULL
  AND (r.[Review_finalized_date] < s.[Sample_start_date] 
       OR r.[Review_finalized_date] >= DATEADD(day,1,s.[Sample_end_date]));


       Thank you, Geoffrey. I truly appreciate your kind words and your confidence in me. It has been a pleasure working with you, and I'm grateful for the opportunity. I'll continue giving my best to help ensure a smooth UAT rollout and successful delivery. Looking forward to working together over the coming weeks.
