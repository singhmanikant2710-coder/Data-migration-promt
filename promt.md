-- 1. Table ka structure dekho (columns + types + nullability)
SELECT COLUMN_NAME, DATA_TYPE, IS_NULLABLE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'tblMainTTMCalculations'
ORDER BY ORDINAL_POSITION;

-- 2. Primary key / unique constraint dekho (UPSERT ke liye zaroori)
SELECT i.name AS index_name, c.name AS column_name, i.is_primary_key, i.is_unique
FROM sys.indexes i
JOIN sys.index_columns ic ON i.object_id = ic.object_id AND i.index_id = ic.index_id
JOIN sys.columns c ON ic.object_id = c.object_id AND ic.column_id = c.column_id
WHERE i.object_id = OBJECT_ID('dbo.tblMainTTMCalculations')
ORDER BY i.name, ic.key_ordinal;

-- 3. Ek customer ka existing data dekho (kaise rows store hain — ek month = ek row?)
SELECT TOP 5 * FROM dbo.tblMainTTMCalculations
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
ORDER BY strMonthKey DESC;
