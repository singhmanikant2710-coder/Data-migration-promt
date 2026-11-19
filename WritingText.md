DECLARE @ReviewId int = 21541;
SELECT 'Review' AS Src, NULL AS Account_id, CAST(r.CCL AS int) AS CCL, CAST(r.Special_assets AS int) AS Special_assets
FROM dbo.[02_CORE_02_Reviews] r WHERE r.Review_id = @ReviewId
UNION ALL
SELECT 'Account', a.Account_id, CAST(a.CCL AS int), CAST(a.Special_assets AS int)
FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = @ReviewId
ORDER BY Src DESC, Account_id;
