SELECT TOP 5 r.Review_id, r.Customer_number
FROM dbo.[02_CORE_02_Reviews] r
WHERE NOT EXISTS (
    SELECT 1 FROM dbo.[02_CORE_06_Policy Exceptions] p
    WHERE p.Review_id = r.Review_id);
