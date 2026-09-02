SELECT c.name AS PK_Column, i.name AS PK_Name
FROM sys.indexes i
JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id
JOIN sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
WHERE i.is_primary_key = 1
  AND i.object_id = OBJECT_ID('dbo.[03_LIBRARY_09_Selections]');

  
