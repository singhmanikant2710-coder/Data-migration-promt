Two files. Do NOT modify anyone's existing logic (including Jothi's). Only the changes below. Use LIVE DB, ignore columns.csv. Do not plan. Just apply.

UAT #112: On the Review Status grid, the "Manager" column must show header "Approver" and display [Review_approver_name] instead of the CRO manager.

DB confirmed: dbo.[02_CORE_02_Reviews] has column [Review_approver_name].

FILE 1 (backend): backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
In ALL SIX bucket methods (GetCompletedDraftsAsync, GetInProgressAsync, GetDistributedAsync, GetFinalizedAsync, GetApprovedAsync, GetUnopenedOrCancelledAsync), the SELECT currently has:
     r.[CRO_manager_name]
at reader index 6, mapped to Manager.
Change ONLY that column in each of the six SELECTs to:
     r.[Review_approver_name]
Keep it at the same position (index 6), keep the same mapping line (var manager = rdr.IsDBNull(6) ? "" : rdr.GetString(6); ... Manager = manager;). Do not change any other column, WHERE clause, ORDER BY, or the DTO property name — the value continues to flow into ReviewStatusRow.Manager.

FILE 2 (frontend): frontend/src/app/review-status/page.tsx
Change the column header text only:
     <th className="text-left font-semibold px-4 py-2">Manager</th>
to:
     <th className="text-left font-semibold px-4 py-2">Approver</th>
Keep the cell rendering {r.manager} unchanged.

Do not change anything else. Run read-only TypeScript diagnostics on the frontend file only.




SELECT [Review_id], [CRO_manager_name], [Review_approver_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 21734;
