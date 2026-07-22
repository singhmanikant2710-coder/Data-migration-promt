SELECT
    [Review_id],
    [Start_date],
    [Cancelled],
    CASE
        WHEN [Cancelled] = 1 THEN 'Cancelled'
        WHEN [Start_date] IS NULL AND ([Cancelled] = 0 OR [Cancelled] IS NULL)
            THEN 'Unopened'
        ELSE 'Other'
    END AS [Expected_Review_Status]
FROM [dbo].[02_CORE_02_Reviews]
WHERE [Review_id] IN (21847, 21846, 21845, 21844, 21843)
ORDER BY [Review_id] DESC;
