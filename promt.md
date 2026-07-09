-- 1) Column existence
SELECT c.name
FROM sys.columns c
WHERE c.object_id = OBJECT_ID(N'[dbo].[01_DATA_01_Data Mart Trial]')
  AND c.name IN (N'Segment', N'Region', N'SpecialtyLine', N'Market')
ORDER BY c.name;

-- 2) Segment distinct
SELECT DISTINCT TOP (20) LTRIM(RTRIM([Segment])) AS Segment
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [Segment] IS NOT NULL AND LTRIM(RTRIM([Segment])) <> ''
ORDER BY Segment;

-- 3) Region distinct
SELECT DISTINCT TOP (20) LTRIM(RTRIM([Region])) AS Region
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [Region] IS NOT NULL AND LTRIM(RTRIM([Region])) <> ''
ORDER BY Region;

-- 4) SpecialtyLine distinct
SELECT DISTINCT TOP (20) LTRIM(RTRIM([SpecialtyLine])) AS SpecialtyLine
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [SpecialtyLine] IS NOT NULL AND LTRIM(RTRIM([SpecialtyLine])) <> ''
ORDER BY SpecialtyLine;

-- 5) Market distinct
SELECT DISTINCT TOP (20) LTRIM(RTRIM([Market])) AS Market
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [Market] IS NOT NULL AND LTRIM(RTRIM([Market])) <> ''
ORDER BY Market;
