SELECT c.name AS ColumnName, t.name AS TypeName
FROM sys.columns c
JOIN sys.types t ON t.user_type_id = c.user_type_id
WHERE c.object_id = OBJECT_ID('dbo.[02_CORE_02_Reviews]')
  AND t.name IN ('timestamp', 'rowversion');


  SET NOCOUNT ON;
GO
/* #232 uses the EXISTING rowversion/timestamp column on dbo.[02_CORE_02_Reviews].
   No schema change. Read-only check for DBA in QA/Prod: expect 1 row named <COL>. */
SELECT c.[name] AS [ColumnName], t.[name] AS [DataType], c.[is_nullable] AS [IsNullable]
FROM sys.columns AS c
JOIN sys.types AS t ON t.[user_type_id] = c.[user_type_id]
WHERE c.[object_id] = OBJECT_ID(N'dbo.[02_CORE_02_Reviews]')
  AND t.[name] IN (N'timestamp', N'rowversion');
GO

SELECT COL_LENGTH('dbo.[02_CORE_02_Reviews]', '<COL>') AS Len;
