SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND (COLUMN_NAME LIKE '%approver%' OR COLUMN_NAME LIKE '%approval%');


  Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #112): On the Review Status grid, the "Manager" column must change its header to "Approver" and display [Review_approver_name] instead of the current manager value.

Report:
1) In frontend/src/app/review-status/page.tsx: the "Manager" column header and the row cell that renders the manager value. Paste both. What field on the row DTO does it read (e.g. r.manager)?
2) In backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs and the ReviewStatusRow DTO: which column currently feeds the "manager" value in each bucket's SELECT? Show the SELECT column and the mapping.
3) Does dbo.[02_CORE_02_Reviews] have a column [Review_approver_name]? If the exact name differs, report the actual approver-name column.
4) State exactly what to change and in how many files: (a) header label Manager -> Approver, (b) row value -> Review_approver_name. Note whether the backend SELECT/DTO must change to return the approver name, or whether only the frontend header needs changing.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
