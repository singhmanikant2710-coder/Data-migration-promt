SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND (COLUMN_NAME LIKE '%EIC%' OR COLUMN_NAME LIKE '%examiner%' OR COLUMN_NAME LIKE '%charge%');

  SELECT [Review_id], [CRO_name], [CRO_manager_name],
       [Relationship_mgr_name], [Portfolio_mgr_name],
       [<EIC column jo mile>]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 21882;
