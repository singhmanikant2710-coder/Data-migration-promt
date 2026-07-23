SELECT [Review_id], [Customer_name], [CRO_name], [Relationship_mgr_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Sample_id] = 363
ORDER BY [Review_id];
