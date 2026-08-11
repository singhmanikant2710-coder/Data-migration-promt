DIAGNOSTIC ONLY — DO NOT EDIT ANY FILE. Read-only investigation. Report findings only, with file paths and line numbers.

Investigate the "Unlock Review" workflow for two confirmed bugs. Trace the full code path from the Unlock Review modal action through to the SQL UPDATE.

Bug 1 — "Unlock for General Revisions" does NOT clear [Review_finalized_date]. These steps work correctly: transfers Review_approval_date into Review_initial_approval_date (first unlock only), clears Review_approval_date, leaves Review_approver_name as-is, sets Locked = FALSE. Only clearing Review_finalized_date is broken.

Bug 2 — "Unlock for Reconsideration" and "Unlock for Appeal" open their sub-forms correctly, but afterward should follow the SAME workflow as General Revisions (including clearing Review_finalized_date). This shared workflow is NOT being applied for these two paths.

Locate and report (no edits):

1. The command/handler/service that processes "Unlock for General Revisions". Show exactly which review fields it sets or clears.

2. Whether Review_finalized_date is included anywhere in that update path — in the C# entity mutation, the repository method, AND the actual SQL UPDATE statement. Confirm at which layer it is missing (set to null in C# but dropped from the SQL column list? or never set at all?).

3. The repository method that issues the SQL UPDATE for unlock. Quote the column list in the UPDATE statement so we can see whether Review_finalized_date is present.

4. The handlers for "Unlock for Reconsideration" and "Unlock for Appeal". Report whether they call the SAME shared unlock logic as General Revisions, or use a separate/duplicated code path. If separate, list exactly which fields each one sets or clears versus the General Revisions path.

5. A summary table: for each of the three unlock options (General Revisions / Reconsideration / Appeal), show which of these fields each currently clears or sets — Review_approval_date, Review_initial_approval_date, Review_approver_name, Locked, Review_finalized_date.

Output findings only. No code changes and no fix suggestions yet — just the trace and the summary table.
