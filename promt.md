SELECT 
    a.[Review_id],
    a.[Account_id],
    a.[Delinquent_status],
    '[' + CAST(a.[Delinquent_status] AS NVARCHAR(50)) + ']' AS RawValue
FROM dbo.[02_CORE_04_Accounts] a
WHERE (a.[Review_id] = 21902 AND a.[Account_id] = 75183)
   OR (a.[Review_id] = 21903 AND a.[Account_id] = 75184)
   OR (a.[Review_id] = 21904 AND a.[Account_id] = 75185)
   OR (a.[Review_id] = 21934 AND a.[Account_id] = 75325)
   OR (a.[Review_id] = 21935 AND a.[Account_id] = 75326)
   OR (a.[Review_id] = 21936 AND a.[Account_id] = 75327)
   OR (a.[Review_id] = 21915 AND a.[Account_id] = 75263)
   OR (a.[Review_id] = 21909 AND a.[Account_id] = 75218);
