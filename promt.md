-- 1. Raw count (abhi report kya dikhata)
SELECT COUNT(*) AS RawCount
FROM dbo.[02_CORE_04_Accounts]
WHERE [Review_id] IN (<sample ke review ids>);

-- 2. De-duped count (Geoff ka rule)
SELECT COUNT(*) AS DedupedCount FROM (
  SELECT DISTINCT [Review_id],[Scorecard_id_bank],[Bank_PD],[Bank_LGD],[CAS_PD],[CAS_LGD]
  FROM dbo.[02_CORE_04_Accounts]
  WHERE [Review_id] IN (<sample ke review ids>)
) d;

-- 3. Bad-data reveal (same Bank ID, alag PD/LGD)
SELECT [Review_id],[Scorecard_id_bank],
       COUNT(DISTINCT CONCAT([Bank_PD],'|',[Bank_LGD],'|',[CAS_PD],'|',[CAS_LGD])) AS DistinctCombos
FROM dbo.[02_CORE_04_Accounts]
WHERE [Review_id] IN (<sample ke review ids>)
GROUP BY [Review_id],[Scorecard_id_bank]
HAVING COUNT(DISTINCT CONCAT([Bank_PD],'|',[Bank_LGD],'|',[CAS_PD],'|',[CAS_LGD])) > 1;
