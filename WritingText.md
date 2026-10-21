SELECT COALESCE(NULLIF(LTRIM(RTRIM(r.CRO_name)),''),'(Unknown)') AS Reviewer,
       COUNT(*) AS Total,
       FORMAT(MIN(r.Completed_date), 'yyyy-MM') AS FirstMonth,
       FORMAT(MAX(r.Completed_date), 'yyyy-MM') AS LastMonth
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE (r.Cancelled IS NULL OR r.Cancelled = 0) AND r.Segment IS NOT NULL
  AND r.Completed_date BETWEEN '2025-07-01T00:00:00' AND '2026-06-30T23:59:59'
GROUP BY COALESCE(NULLIF(LTRIM(RTRIM(r.CRO_name)),''),'(Unknown)')
ORDER BY 1;
