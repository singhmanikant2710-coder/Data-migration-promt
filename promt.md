SELECT [Selection], [Selection_id]
FROM [dbo].[03_LIBRARY_09_Selections] WITH (NOLOCK)
WHERE Tab = 'Customer Info' AND Section = 'FHN Portfolio Segment'
ORDER BY [Selection_id];
