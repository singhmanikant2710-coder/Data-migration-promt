SELECT TOP 5 Review_id, Customer_name, Sample_id
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
ORDER BY Review_id DESC;

SELECT Review_id, Customer_name,
       Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email,
       Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_id = <naya Review_id>;
