SELECT c.name AS ColumnName
FROM sys.columns c
WHERE c.object_id = OBJECT_ID(N'[dbo].[01_DATA_01_Data Mart Trial]')
  AND c.name LIKE '%IntRep%';

  SELECT COUNT(*) AS TotalRows
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK);

SELECT TOP 20 [IntRepCMLSubCategory]
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK);
