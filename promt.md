-- Kaunse samples mein aise reviews hain jo Completed hain par approved nahi
-- (jahan fix ka visible difference dikhega)
SELECT r.[Sample_id], COUNT(*) AS CompletedNotApproved
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
  AND r.[Completed_date] IS NOT NULL
  AND r.[Review_approval_date] IS NULL
  AND a.[Bank_PD] BETWEEN 1 AND 16
  AND a.[CAS_PD] BETWEEN 1 AND 16
GROUP BY r.[Sample_id]
HAVING COUNT(*) > 0
ORDER BY CompletedNotApproved DESC;
