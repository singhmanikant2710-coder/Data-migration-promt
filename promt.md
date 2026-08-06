-- Sample ke liye actual commitment total (raw dollars)
SELECT SUM(a.[Commitment]) / 1000000.0 AS ActualTotalMM
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE r.[Sample_id] = <sample>
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND a.[Bank_PD] BETWEEN 1 AND 16
  AND a.[CAS_PD] BETWEEN 1 AND 16
  AND r.[Completed_date] IS NOT NULL;  -- ya jo status select kiya
