-- Regional Banking ke Units (Region se)
SELECT DISTINCT LTRIM(RTRIM([Region])) AS Unit
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE LTRIM(RTRIM([Segment])) = 'Regional Banking'
  AND [Region] IS NOT NULL AND LTRIM(RTRIM([Region])) <> ''
ORDER BY Unit;

-- Wholesale (non-Regional) ke Units (SpecialtyLine se)
SELECT DISTINCT LTRIM(RTRIM([SpecialtyLine])) AS Unit
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE LTRIM(RTRIM([Segment])) = 'Wholesale'
  AND [SpecialtyLine] IS NOT NULL AND LTRIM(RTRIM([SpecialtyLine])) <> ''
ORDER BY Unit;
