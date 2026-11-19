SELECT f.Review_id, r.Customer_name, r.Sample_id, f.Finding_code, f.Finding_level, r.Completed_date
FROM dbo.[02_CORE_07_Findings] f WITH (NOLOCK)
JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.Review_id = f.Review_id
WHERE LTRIM(RTRIM(f.Finding_level)) = 'Low';

Dev data: Finding_level values are Observation (2690), Finding (697) and Low (7).
Change for the CRM Summary report only: in SqlCrmSummaryReportRepository.PopulateFindingsAsync, when Finding_level is not Finding / Key Finding / Observation, return the trimmed stored value (e.g. "Low") instead of null; NULL/blank stays empty. Check that "Severity" from this repository is not used by any other report or logic (file:line) before changing. PDF and Excel then show the same value.
Report CHANGED (file:line), dotnet build, npx tsc --noEmit, npm run build, git status. Do not commit or push.
