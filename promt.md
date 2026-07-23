DECLARE @sql NVARCHAR(MAX) = '';
SELECT @sql = @sql + 
  'SELECT ''' + COLUMN_NAME + ''' AS ColumnName, COUNT([' + COLUMN_NAME + ']) AS NonNullCount FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK) UNION ALL '
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '01_DATA_01_Data Mart Trial';

SET @sql = LEFT(@sql, LEN(@sql) - 10);
SET @sql = 'SELECT * FROM (' + @sql + ') t WHERE NonNullCount = 0 ORDER BY ColumnName;';

EXEC sp_executesql @sql;


SELECT 
  COUNT([InternalPortCat])      AS InternalPortCat,
  COUNT([IntRepCMLSubCategory]) AS IntRepCMLSubCategory
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK);


Frontend only. Single file: frontend/src/app/review-queue/page.tsx
Do NOT modify any other file. Do not plan. Just apply.

UAT #141 follow-up (client feedback):
1. The Sample Name dropdown must list only OPEN samples ([02_CORE_01_Samples].[Closed] = No / 0).
2. Its border colour must match the "My View" filter control on the left.

Changes:

a) Sample options source: the page currently populates sampleOptions via searchSamples({ page, pageSize }) which returns ALL samples. Add the closed filter so only open samples are returned:
   searchSamples({ page, pageSize, closed: false })
   The SampleSearchParams type already supports `closed?: boolean`. Do not change anything else in that effect.

b) Border colour: update the Sample Name <select> className so its border matches the "My View" select exactly. Read the My View select's current className in this file and apply the same border classes to the Sample Name select. Do not change the My View control itself.

Do NOT change the search box, page size, the load effect, or getReviewQueuePage.

Run read-only TypeScript diagnostics on this file only.
