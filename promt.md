
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '01_DATA_01_Data Mart Trial'
  AND (COLUMN_NAME LIKE '%InternalPortCat%'
    OR COLUMN_NAME LIKE '%IntRepCMLSubCategory%'
    OR COLUMN_NAME LIKE '%PortCat%'
    OR COLUMN_NAME LIKE '%SubCateg%');

    SELECT 
  COUNT(*) AS TotalRows,
  COUNT([InternalPortCat])       AS InternalPortCat_HasData,
  COUNT([IntRepCMLSubCategory])  AS IntRepCMLSubCategory_HasData
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK);

SELECT TOP 5 [InternalPortCat], [IntRepCMLSubCategory]
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [InternalPortCat] IS NOT NULL;
