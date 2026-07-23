SELECT TOP 5 [Review_id], [Sample_id], [Customer_name], [CRO_name], [Relationship_mgr_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [CRO_name] IS NULL
  AND [Relationship_mgr_name] IS NOT NULL
ORDER BY [Review_id] DESC;
