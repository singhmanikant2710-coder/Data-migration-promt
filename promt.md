READ-ONLY SQL VERIFICATION. Run these SELECTs only — do NOT modify any data or code. Just show me the numbers.

Use a sample review set (e.g. the Review_ids for sample 357 or whichever is currently tested).

-- 1. Current raw account count (what report shows now)
SELECT COUNT(*) AS RawCount
FROM dbo.[02_CORE_04_Accounts]
WHERE [Review_id] IN (<sample review ids>);

-- 2. De-duped count per Geoff's rule (Review_id + Scorecard_id_bank + all PD/LGD)
SELECT COUNT(*) AS DedupedCount FROM (
  SELECT DISTINCT [Review_id], [Scorecard_id_bank], [Bank_PD], [Bank_LGD], [CAS_PD], [CAS_LGD]
  FROM dbo.[02_CORE_04_Accounts]
  WHERE [Review_id] IN (<sample review ids>)
) d;

-- 3. Show any cases where same Review_id + Scorecard_id_bank has DIFFERING PD/LGD (the bad-data reveal Geoff mentioned)
SELECT [Review_id], [Scorecard_id_bank], COUNT(DISTINCT CONCAT([Bank_PD],'|',[Bank_LGD],'|',[CAS_PD],'|',[CAS_LGD])) AS DistinctRatingCombos
FROM dbo.[02_CORE_04_Accounts]
WHERE [Review_id] IN (<sample review ids>)
GROUP BY [Review_id], [Scorecard_id_bank]
HAVING COUNT(DISTINCT CONCAT([Bank_PD],'|',[Bank_LGD],'|',[CAS_PD],'|',[CAS_LGD])) > 1;

Show me the three results. No edits.
