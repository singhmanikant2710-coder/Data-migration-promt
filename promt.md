Two files. Do NOT modify anyone's existing logic (including Jothi's). Only the two specific changes below. Use LIVE DB, ignore columns.csv. Do not plan. Just apply.

UAT #111: On the Review Status grid, the "Completed" column must dynamically change its label based on the selected bucket, and In Progress rows must show the Start_date.

FILE 1 (backend): backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
In the In Progress bucket method (GetInProgressAsync), the SELECT already includes r.[Start_date] at index 11, but the mapping currently sets Completed = "". Change ONLY that mapping to format the Start_date into Completed, exactly like the other buckets do:
     var startDt = rdr.IsDBNull(11) ? (DateTime?)null : rdr.GetDateTime(11);
     ...
     Completed = startDt.HasValue ? startDt.Value.ToString("M/d/yyyy", us) : ""
Do not change the SQL, the WHERE clause, or any other bucket method.

FILE 2 (frontend): frontend/src/app/review-status/page.tsx
Replace the static "Completed" column header with a dynamic label derived from selectedBucket:
     const completedColLabel =
       selectedBucket === "In Progress"     ? "Started" :
       selectedBucket === "Draft Completed" ? "Completed" :
       selectedBucket === "Approved"        ? "Approved" :
       selectedBucket === "Distributed"     ? "Distributed" :
       selectedBucket === "Finalized"       ? "Finalized" :
       "Completed";   // default for All Statuses / Unopened-Cancelled
Then use {completedColLabel} in place of the hardcoded "Completed" in the column <th>. Keep the cell rendering as {r.completed} unchanged.

Do not change any other column, the bucket filter logic, the counts, or anything else.

Run read-only TypeScript diagnostics on the frontend file only.
