SELECT 
  COUNT([Segment]) AS SegmentNonNull,
  COUNT([Region]) AS RegionNonNull,
  COUNT([SpecialtyLine]) AS SpecialtyNonNull,
  COUNT([Market]) AS MarketNonNull
FROM [dbo].[01_DATA_01_Data Mart Trial] WITH (NOLOCK);
