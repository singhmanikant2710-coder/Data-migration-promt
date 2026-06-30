SELECT COUNT(*) 
FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK)
WHERE [Review_id] = 21541;

SELECT TOP 20 [Review_id], COUNT(*) AS finding_count
FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK)
GROUP BY [Review_id]
ORDER BY finding_count DESC;

SELECT COUNT(*) FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK);
