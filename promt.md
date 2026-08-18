Single-file edit: backend/src/Casrr.Infrastructure/SqlServer/SqlScorecardResultsReportRepository.cs

Fix (Item 4, Geoff-approved): De-duplicate the Scorecard Details rows by Review_id + Scorecard_id_bank + PD/LGD combo. Identical combos collapse to one row (keeping the latest); rows with the same Bank ID but DIFFERING PD/LGD stay separate (to reveal bad data). Verified on Review_ids 21592,21574: raw 6 -> deduped 3, no differing-PD/LGD cases.

Wrap the current PopulateScorecardsAsync SELECT in a CTE using ROW_NUMBER() to keep one row per unique key, ordered by the existing tie-breaker (latest scorecard_date, then transaction_id).

BEFORE:
SELECT a.[Review_id],
       a.[Facility_number],
       a.[Scorecard_id_system],
       a.[Scorecard_id_bank],
       a.[Scorecard_date],
       a.[Scorecard_type],
       a.[Bank_PD],
       a.[CAS_PD],
       a.[Bank_LGD],
       a.[CAS_LGD],
       a.[Scorecard_assessment]
FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
WHERE a.[Review_id] IN (__IDS__)
ORDER BY a.[Review_id], a.[Scorecard_date] DESC, a.[Scorecard_transaction_id] DESC, a.[Scorecard_id_bank];

AFTER:
WITH Deduped AS (
  SELECT a.[Review_id],
         a.[Facility_number],
         a.[Scorecard_id_system],
         a.[Scorecard_id_bank],
         a.[Scorecard_date],
         a.[Scorecard_type],
         a.[Bank_PD],
         a.[CAS_PD],
         a.[Bank_LGD],
         a.[CAS_LGD],
         a.[Scorecard_assessment],
         ROW_NUMBER() OVER (
           PARTITION BY a.[Review_id], a.[Scorecard_id_bank], a.[Bank_PD], a.[Bank_LGD], a.[CAS_PD], a.[CAS_LGD]
           ORDER BY a.[Scorecard_date] DESC, a.[Scorecard_transaction_id] DESC
         ) AS rn
  FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
  WHERE a.[Review_id] IN (__IDS__)
)
SELECT [Review_id],
       [Facility_number],
       [Scorecard_id_system],
       [Scorecard_id_bank],
       [Scorecard_date],
       [Scorecard_type],
       [Bank_PD],
       [CAS_PD],
       [Bank_LGD],
       [CAS_LGD],
       [Scorecard_assessment]
FROM Deduped
WHERE rn = 1
ORDER BY [Review_id], [Scorecard_date] DESC, [Scorecard_id_bank];

CONSTRAINTS:
- Keep the exact same output columns and order as before (so the existing row mapping code works unchanged).
- Do NOT change ComputeTotalsAsync in this edit (next step).
- Do NOT change any other query, method, or file.
- Preserve how __IDS__ is expanded (same placeholder mechanism).
- Show the diff.
