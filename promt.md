SELECT 
  COUNT(*) AS Total,
  COUNT([IntRepCMLSubCategory]) AS NonNull
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK);
