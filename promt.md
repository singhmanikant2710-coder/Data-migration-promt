SELECT COUNT(*) AS OldApprovedGated
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND r.[Sample_id] = 354
  AND r.[Review_approval_date] IS NOT NULL
  AND a.[Bank_PD] BETWEEN 1 AND 16
  AND a.[CAS_PD] BETWEEN 1 AND 16;
