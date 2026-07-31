-- Account-level count (jo report abhi use karta hai)
SELECT COUNT(*) AS AccountRowCount
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_04_Accounts] a ON a.[Review_id] = r.[Review_id]
WHERE r.[Sample_id] = 356;

-- Scorecard-level count (agar alag table hai)
SELECT COUNT(*) AS ScorecardRowCount
FROM dbo.[02_CORE_02_Reviews] r
INNER JOIN dbo.[02_CORE_03_Scorecards] sc ON sc.[Review_id] = r.[Review_id]
WHERE r.[Sample_id] = 356;
