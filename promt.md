SELECT
    r.[Sample_id],
    r.[Review_id],
    r.[Customer_name],
    SUM(COALESCE(a.[Commitment], 0)) AS Review_commitment
FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
INNER JOIN dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
    ON a.[Review_id] = r.[Review_id]
WHERE r.[Sample_id] = 357
GROUP BY
    r.[Sample_id],
    r.[Review_id],
    r.[Customer_name]
ORDER BY
    r.[Review_id];
