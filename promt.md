SELECT TOP 5 Review_id, Relationship_mgr_number, Relationship_mgr_name
FROM dbo.[02_CORE_02_Reviews]
WHERE Relationship_mgr_number LIKE '0%' AND Relationship_mgr_number <> '00000';


SELECT Review_id, Portfolio_mgr_number, Portfolio_mgr_name
FROM dbo.[02_CORE_02_Reviews]
WHERE Portfolio_mgr_number = '00000' AND Portfolio_mgr_name IS NOT NULL;


SELECT Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email,
       Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email
FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = 21592;
