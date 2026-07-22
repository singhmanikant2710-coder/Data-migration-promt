DECLARE @sql NVARCHAR(MAX) = '';
SELECT @sql = @sql + 
  'SELECT ''' + COLUMN_NAME + ''' AS ColumnName, COUNT([' + COLUMN_NAME + ']) AS NonNullCount FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK) UNION ALL '
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '01_DATA_01_Data Mart Trial';

SET @sql = LEFT(@sql, LEN(@sql) - 10); -- remove trailing UNION ALL
SET @sql = 'SELECT * FROM (' + @sql + ') t WHERE NonNullCount = 0 ORDER BY ColumnName;';

EXEC sp_executesql @sql;

SELECT [Review_id], [CRO_name], [Relationship_mgr_name], [Reviewer_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Sample_id] = 363;
