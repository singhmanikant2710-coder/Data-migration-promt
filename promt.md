SELECT TOP 5 r.Review_id, r.Relationship_mgr_number, r.Relationship_mgr_name
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.Relationship_mgr_name NOT LIKE '%,%'
  AND EXISTS (SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] dp
              WHERE TRY_CONVERT(int, dp.Recipient_role) = TRY_CONVERT(int, r.Relationship_mgr_number));


SELECT TOP 5 r.Review_id, r.Relationship_mgr_number, r.Relationship_mgr_name
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.Relationship_mgr_number IS NOT NULL AND r.Relationship_mgr_number <> '00000'
  AND NOT EXISTS (SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] dp
                  WHERE TRY_CONVERT(int, dp.Recipient_role) = TRY_CONVERT(int, r.Relationship_mgr_number));


SELECT Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email
FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = 21592;
