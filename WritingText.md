SELECT TOP 5 Review_id, Customer_name, Sample_id
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE CAST(Risk_rating_justification AS nvarchar(max)) LIKE '%<img%'
   OR CAST(Risk_rating_justification AS nvarchar(max)) LIKE '%<li%';
