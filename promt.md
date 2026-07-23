Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #127, EIC part — clarified by client): The "Examiner in Charge" field on Review Info must display the EIC assigned to the review's SAMPLE, sourced from dbo.[02_CORE_01_Samples].[EIC_Name]. It currently derives from the CRO/Reviewer name, so it shows the wrong person when a CRO exists, and blank when no CRO is assigned. EIC is assigned on every new sample (e.g. Sample 354 EIC_Name = 'Kim Doane'), though older samples may have it NULL.

Report:
1) In backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs — the SELECT that builds the review header / review-info section (GetReviewHeaderByIdAsync and the related header queries). Does it already join dbo.[02_CORE_01_Samples]? Paste the FROM/JOIN clauses and show whether [EIC_Name] is available there.
2) Paste the backend ReviewInfoSection contract showing the existing assignment fields (ReviewerName, ReviewerEmail, ManagerName, ManagerEmail), and state where a new ExaminerInCharge field would be added.
3) In frontend/src/services/api/reviews.ts — paste the ReviewInfoSection type showing the same fields.
4) In useReviewInfo.ts — paste the current examinerInCharge mapping, and show where it is consumed in ReviewInfoSection.tsx.
5) State exactly what must change and in how many files so the backend returns the sample's EIC_Name as examinerInCharge and the UI displays it (blank only when the sample has no EIC).

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
