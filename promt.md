
Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #160): On Review History → Review List, add two columns — "Review ID" and "Customer Number" — placed between the existing "Sample Name" and "Borrower Name" columns.

Report:
1) The Review History page component (likely frontend/src/app/review-history/page.tsx). Paste the grid column definitions, showing the current column order — specifically the "Sample Name" and "Borrower Name / Linesheet" columns and what sits between them. What field does each column bind to on the row?
2) Paste the Review History row type (the TypeScript type for a row) from the API service (frontend/src/services/api/...). Does the row already include reviewId and customerNumber, or only some fields?
3) In the backend Review History repository (SqlReviewHistoryRepository.cs or equivalent), paste the SELECT that builds each row. Are [Review_id] and [Customer_number] already selected, or would they need to be added? Show the mapping to the row DTO.
4) State exactly what must change and in how many files to add the two columns in the correct position, noting whether the backend needs to return reviewId / customerNumber or whether they're already present.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
