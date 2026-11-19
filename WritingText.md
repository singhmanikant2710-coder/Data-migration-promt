SELECT '[' + ISNULL(Finding_level, '<NULL>') + ']' AS LevelRaw, COUNT(*) AS Cnt
FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK)
GROUP BY Finding_level ORDER BY Cnt DESC;
