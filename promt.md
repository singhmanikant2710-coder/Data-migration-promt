SELECT
  SUM(CASE WHEN (Relationship_mgr_number IS NULL OR Relationship_mgr_number = '00000')
            AND Relationship_mgr_name IS NOT NULL THEN 1 ELSE 0 END) AS RM_NameOnly,
  SUM(CASE WHEN (Portfolio_mgr_number IS NULL OR Portfolio_mgr_number = '00000')
            AND Portfolio_mgr_name IS NOT NULL THEN 1 ELSE 0 END) AS PM_NameOnly
FROM dbo.[02_CORE_02_Reviews];
