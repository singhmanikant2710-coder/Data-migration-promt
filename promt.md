SELECT DISTINCT LTRIM(RTRIM([Market])) AS Market
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE LTRIM(RTRIM([Segment])) = 'Wholesale'
  AND LTRIM(RTRIM([SpecialtyLine])) = 'Corporate Correspondent'
  AND [Market] IS NOT NULL AND LTRIM(RTRIM([Market])) <> ''
ORDER BY Market;

SELECT DISTINCT LTRIM(RTRIM([Market])) AS Market
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE LTRIM(RTRIM([Segment])) = 'Regional Banking'
  AND LTRIM(RTRIM([Region])) = 'South'
  AND [Market] IS NOT NULL AND LTRIM(RTRIM([Market])) <> ''
ORDER BY Market;
