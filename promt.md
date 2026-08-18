Single-file edit: backend/src/Casrr.Infrastructure/SqlServer/SqlScorecardResultsReportRepository.cs

Fix (Item 3, Geoff-approved): The summary count currently uses COUNT(*) (raw account rows). Change it to count the SAME de-duped population as the details (Item 4), so totals reconcile with the rows shown. De-dup key: Review_id + Scorecard_id_bank + Bank_PD + Bank_LGD + CAS_PD + CAS_LGD. Verified: raw 6 -> deduped 3 for Review_ids 21592,21574.

Wrap in a CTE using ROW_NUMBER() (same key + tie-breaker as the details query), keep rn=1, then GROUP BY Scorecard_assessment and count.

BEFORE:
SELECT a.[Scorecard_assessment] AS [Assessment],
       COUNT(*) AS [Cnt]
FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
WHERE a.[Review_id] IN (__IDS__)
GROUP BY a.[Scorecard_assessment]
ORDER BY a.[Scorecard_assessment];

AFTER:
WITH Deduped AS (
  SELECT a.[Scorecard_assessment],
         ROW_NUMBER() OVER (
           PARTITION BY a.[Review_id], a.[Scorecard_id_bank], a.[Bank_PD], a.[Bank_LGD], a.[CAS_PD], a.[CAS_LGD]
           ORDER BY a.[Scorecard_date] DESC, a.[Scorecard_transaction_id] DESC
         ) AS rn
  FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
  WHERE a.[Review_id] IN (__IDS__)
)
SELECT [Scorecard_assessment] AS [Assessment],
       COUNT(*) AS [Cnt]
FROM Deduped
WHERE rn = 1
GROUP BY [Scorecard_assessment]
ORDER BY [Scorecard_assessment];

CONSTRAINTS:
- Use the EXACT same PARTITION BY key and ORDER BY tie-breaker as the details de-dup (Fix 1), so totals reconcile with detail rows.
- Keep output columns [Assessment], [Cnt] identical (so mapping code is unchanged).
- Preserve __IDS__ expansion.
- Do NOT change the details query (already done) or any other method/file.
- Show the diff.
