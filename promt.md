SELECT Review_id, Relationship_mgr_number, Relationship_mgr_name
FROM dbo.[02_CORE_02_Reviews]
WHERE LTRIM(RTRIM(Relationship_mgr_name)) = 'JOHN C WAGNER II'
  AND (TRY_CONVERT(int, Relationship_mgr_number) <> 17436 OR Relationship_mgr_number IS NULL);
