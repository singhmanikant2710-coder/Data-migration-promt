
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

SELECT 
  COUNT([InternalPortcat])  AS InternalPortcat_smallc,
  COUNT([PortCat])          AS PortCat,
  COUNT([PortCat1])         AS PortCat1,
  COUNT([PortCat2])         AS PortCat2,
  COUNT([PortCat3])         AS PortCat3,
  COUNT([ExtRepCMLSubCategory]) AS ExtRep,
  COUNT([IntRepCMLSubCategory]) AS IntRep
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK);
