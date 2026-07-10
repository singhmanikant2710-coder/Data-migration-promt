SELECT DISTINCT LTRIM(RTRIM([IntRepCMLSubCategory])) AS Val
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [IntRepCMLSubCategory] IS NOT NULL 
  AND LTRIM(RTRIM([IntRepCMLSubCategory])) <> ''
ORDER BY Val;

SELECT COUNT(DISTINCT LTRIM(RTRIM([IntRepCMLSubCategory]))) AS DistinctCount
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [IntRepCMLSubCategory] IS NOT NULL 
  AND LTRIM(RTRIM([IntRepCMLSubCategory])) <> '';
