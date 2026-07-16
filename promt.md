SELECT DISTINCT [OfficerNumber], [OfficerName]
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [OfficerName] IS NOT NULL AND [OfficerNumber] IS NOT NULL
ORDER BY [OfficerName];

SELECT DISTINCT [PM Number], [PMName]
FROM dbo.[01_DATA_01_Data Mart Trial] WITH (NOLOCK)
WHERE [PMName] IS NOT NULL AND [PM Number] IS NOT NULL
ORDER BY [PMName];

SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND COLUMN_NAME IN ('Relationship_mgr_name','Relationship_mgr_number','Portfolio_mgr_name','Portfolio_mgr_number');
