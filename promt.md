SELECT 
    COUNT(*) AS total_rows,
    COUNT(CASE WHEN curPrincipalNRPriorMonth > 0 THEN 1 END) AS prior_populated,
    COUNT(CASE WHEN curPrincipalNRPriorMonth = 0 OR curPrincipalNRPriorMonth IS NULL THEN 1 END) AS prior_zero
FROM tblMain
WHERE curCashCollections > 0;   -- jinme cash collections hai
