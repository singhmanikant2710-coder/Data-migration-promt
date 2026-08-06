SELECT DISTINCT r.[Sample_id], a.[Bank_PD], a.[CAS_PD]
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE (a.[Bank_PD] BETWEEN 13 AND 16 OR a.[CAS_PD] BETWEEN 13 AND 16)
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
ORDER BY r.[Sample_id];
