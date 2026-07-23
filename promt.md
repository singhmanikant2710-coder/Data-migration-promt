SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_01_Samples'
  AND COLUMN_NAME LIKE '%arget%';

  SELECT [Sample_id], [Sample_name], [EIC_Name], [<Target column>]
FROM dbo.[02_CORE_01_Samples] WITH (NOLOCK)
WHERE [Sample_id] IN (354, 363, 371);

Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #148 item 1): On Review Form → Review Info → Review Details, the "SAMPLE TARGET" field is blank. It should display dbo.[02_CORE_01_Samples].[Sample Target] for the review's sample.

Note: the three header queries in SqlReviewRepository.cs (GetReviewHeaderByIdAsync, GetLatestReviewHeaderForSampleAndEcifAsync, GetLatestReviewHeaderForEcifAsync) already have a LEFT JOIN to dbo.[02_CORE_01_Samples] AS smp (added recently for EIC_Name).

Report:
1) In the ReviewInfoSection contract (backend) and in frontend/src/services/api/reviews.ts — is there already a SampleTarget / sampleTarget field? Paste the relevant lines.
2) In SqlReviewRepository.cs — is [Sample Target] (or equivalent) already selected in any of the three header queries? Paste the SELECT lines for the sample-related columns, and show how SampleTarget (if present) is currently mapped into ReviewInfoSection.
3) In frontend ReviewInfoSection.tsx — paste the JSX for the "SAMPLE TARGET" field and show which field it binds to.
4) In useReviewInfo.ts — paste the current sampleTarget mapping.
5) State exactly what must change and in how many files to populate SAMPLE TARGET from the sample's Target column, reusing the existing smp join.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
