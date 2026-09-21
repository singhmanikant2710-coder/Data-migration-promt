DECLARE @val float = 43469196;
DECLARE @sql nvarchar(max) = N'';

SELECT @sql = @sql + N'
IF EXISTS (SELECT 1 FROM ' + QUOTENAME(t.TABLE_SCHEMA) + N'.' + QUOTENAME(t.TABLE_NAME) +
  N' WHERE ' + QUOTENAME(c.COLUMN_NAME) + N' = @val)
SELECT ''' + t.TABLE_SCHEMA + '.' + t.TABLE_NAME + ''' AS TableName, ''' + c.COLUMN_NAME + ''' AS ColumnName;'
FROM INFORMATION_SCHEMA.COLUMNS c
JOIN INFORMATION_SCHEMA.TABLES t
  ON c.TABLE_NAME = t.TABLE_NAME AND c.TABLE_SCHEMA = t.TABLE_SCHEMA
WHERE c.DATA_TYPE IN ('float','decimal','money','numeric','real')
  AND t.TABLE_TYPE = 'BASE TABLE';

EXEC sp_executesql @sql, N'@val float', @val = @val;
