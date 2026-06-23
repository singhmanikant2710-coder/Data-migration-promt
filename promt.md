-- Sample 357 mein kitne reviews hain aur unki status dates kya hain
SELECT 
    COUNT(*) AS total_reviews,
    SUM(CASE WHEN Review_finalized_date IS NOT NULL THEN 1 ELSE 0 END) AS finalized,
    SUM(CASE WHEN Completed_date IS NOT NULL THEN 1 ELSE 0 END) AS completed,
    SUM(CASE WHEN Start_date IS NOT NULL THEN 1 ELSE 0 END) AS started,
    SUM(CASE WHEN Cancelled = 1 THEN 1 ELSE 0 END) AS cancelled
FROM dbo.[02_CORE_02_Reviews]
WHERE Sample_id = 357;

-----------

SELECT TOP 20 Sample_id, COUNT(*) AS review_count
FROM dbo.[02_CORE_02_Reviews]
WHERE Cancelled IS NULL OR Cancelled = 0
GROUP BY Sample_id
ORDER BY review_count DESC;
