SELECT
    SUM(CASE WHEN r.[Completed_date] IS NULL THEN 1 ELSE 0 END) AS Finalized_no_completed_date,
    SUM(CASE WHEN r.[Segment] IS NULL THEN 1 ELSE 0 END) AS Finalized_no_segment
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE r.[Review_finalized_date] IS NOT NULL
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0);
