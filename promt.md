SELECT r.Review_id, r.Sample_id, r.Market,
       (SELECT COUNT(*) FROM dbo.[02_CORE_04_Accounts] a WHERE a.Review_id = r.Review_id) AS AccountCount
FROM dbo.[02_CORE_02_Reviews] r
WHERE LTRIM(RTRIM(r.Customer_number)) = '<customer number>';
