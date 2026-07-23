Backend only. Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only ADD. Every existing field must keep behaving exactly as it does. Do not plan. Just apply.

UAT #127 (EIC part): The Review Info "Examiner in Charge" must come from dbo.[02_CORE_01_Samples].[EIC_Name] for the review's sample. Currently it is not returned by the backend at all.

1) ReviewInfoSection contract (find the file where ReviewInfoSection is defined — search for "ReviewerEmail" and "ManagerEmail" properties).
   ADD one nullable property directly after ManagerEmail:
     public string? ExaminerInCharge { get; init; }
   Do not rename, remove, or reorder any existing property.

2) backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs
   a) In the private ReviewHeader record/class, ADD:
        public string? EicName { get; init; }

   b) In ALL THREE header query methods — GetReviewHeaderByIdAsync, GetLatestReviewHeaderForSampleAndEcifAsync, GetLatestReviewHeaderForEcifAsync — do the following in each:
      - ADD this join (place it after the existing FROM/joins, before the WHERE):
          LEFT JOIN dbo.[02_CORE_01_Samples] AS smp WITH (NOLOCK)
              ON smp.[Sample_id] = r.[Sample_id]
      - APPEND this column to the END of the SELECT list (do not reorder existing columns):
          smp.[EIC_Name]
      - Read it DBNull-safe into the new property using a NAME-BASED ordinal lookup (not a hardcoded index):
          var ordEic = rdr.GetOrdinal("EIC_Name");
          ...
          EicName = rdr.IsDBNull(ordEic) ? null : Convert.ToString(rdr.GetValue(ordEic), us)
      Do not change any existing column, WHERE clause, ORDER BY, or existing reader index.

   c) In BOTH GetReviewByEcifAsync and GetReviewByKeysAsync, where ReviewInfoSection is constructed (alongside ReviewerName = row?.CroName etc.), ADD:
        ExaminerInCharge = row?.EicName,
      Do not change any other mapping.

Do not touch the frontend in this step. Report the files changed and the exact new SELECT column line and join.
