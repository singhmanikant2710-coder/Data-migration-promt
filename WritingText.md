SELECT TOP 5 r.Review_id, r.Sample_id, r.Customer_name, COUNT(a.Account_id) AS Accounts
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
JOIN dbo.[02_CORE_04_Accounts] a ON a.Review_id = r.Review_id
WHERE ISNULL(r.Locked,0) = 0 AND ISNULL(r.Cancelled,0) = 0 AND ISNULL(s.Closed,0) = 0
  AND r.CCL = 0
GROUP BY r.Review_id, r.Sample_id, r.Customer_name
HAVING COUNT(a.Account_id) >= 2 AND SUM(CAST(a.CCL AS int)) = 0;

DECLARE @ReviewId int = <RID>;
SELECT 'Review' AS Src, NULL AS Account_id, CAST(r.CCL AS int) AS CCL, CAST(r.Special_assets AS int) AS Special_assets
FROM dbo.[02_CORE_02_Reviews] r WHERE r.Review_id = @ReviewId
UNION ALL
SELECT 'Account', a.Account_id, CAST(a.CCL AS int), CAST(a.Special_assets AS int)
FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = @ReviewId
ORDER BY Src DESC, Account_id;
