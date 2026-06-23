-- Kuch samples lo aur dekho: naam ka number vs Sample_id vs Reviews count
SELECT TOP 10
    s.Sample_id              AS samples_table_id,
    s.Sample_name,
    (SELECT COUNT(*) FROM dbo.[02_CORE_02_Reviews] r 
     WHERE r.Sample_id = s.Sample_id) AS reviews_matching_samplesId
FROM dbo.[02_CORE_01_Samples] s
ORDER BY s.Sample_id DESC;

-- Reviews table mein jo Sample_id values hain, woh kya range mein hain (357 jaisi badi, ya 136 jaisi choti)?
SELECT DISTINCT TOP 20 Sample_id 
FROM dbo.[02_CORE_02_Reviews] 
ORDER BY Sample_id DESC;
