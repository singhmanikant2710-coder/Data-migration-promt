-- 1. Samples table me kitne rows hain (status dropdown isi se bharta hai)
SELECT COUNT(*) AS total_samples FROM dbo.[02_CORE_01_Samples];

-- 2. History dropdown agar finalized-reviews wale samples filter karta hai:
--    Reviews me kitne DISTINCT Sample_id hain jinme finalized data hai
SELECT COUNT(DISTINCT Sample_id) AS distinct_finalized_samples
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_finalized_date IS NOT NULL
  AND (Cancelled IS NULL OR Cancelled = 0);

-- 3. Dono tables ke Sample_id directly match karte hain ya nahi (Bug 1 wala mismatch)
SELECT TOP 20
  s.Sample_id   AS samples_id,
  s.Sample_name,
  r.Sample_id   AS reviews_id
FROM dbo.[02_CORE_01_Samples] s
LEFT JOIN dbo.[02_CORE_02_Reviews] r
  ON r.Sample_id = s.Sample_id
ORDER BY s.Sample_id DESC;
