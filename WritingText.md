UAT #262 – Reports Home filters: Approved status, CAS Manager and Approving Manager filters, layout changes

REQUIREMENT (Geoff)
1. Functional: add "Approved" to the Review Status filter. Dropdown order: Completed > Approved > Distributed > Finalized. Must apply to ALL CRM reports.
2. Visual: move SPECIAL ASSETS MANAGED and CENTRALIZED COMMERCIAL (CCL) below PORTFOLIO INDUSTRY in the Portfolio Filters column.
3. Visual: move CREDIT REVIEW OFFICER to the top of the General Filters list.
4. Functional: add a "CAS MANAGER" filter below CREDIT REVIEW OFFICER.
5. Functional: add an "APPROVING MANAGER" filter below CAS MANAGER.
Geoff's notes from Issue 252 comments: <PASTE Geoff's comment text here, or "not available">

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Reports Home filter UI with file:line (reports/page.tsx): how General / Portfolio filter columns are built, how the form serializes filters (ReportFilters.cs contract), and where the Review Status options come from.
- Review Status: SqlReviewStatusHelper (ReviewStatusPredicate) with file:line – how Completed / Distributed / Finalized map to columns and the Start/End date window. List EVERY report repository that uses it and every report that filters status its own way (all must get Approved). Approved should use dbo.[02_CORE_02_Reviews].Review_approval_date – confirm.
- CAS Manager and Approving Manager: which Reviews columns hold them (e.g. CRO_manager / CRO_manager_email for the CAS manager; Review_approver / Approver name for the approving manager – confirm exact names and sample values in Dev with a read-only SQL). Where would dropdown values come from (distinct values in Reviews, 03_LIBRARY_05_CAS Users, other)? How the existing CREDIT REVIEW OFFICER filter is sourced and applied (copy that pattern).
- Where filters are echoed: PDF "Applied Report Filters" (pageSetup.ts buildFilterParagraph / normalizeFilterValues), the inline filter strings in Scorecard Results and CRO Production PDFs, and the #256 Excel Filters sheet.
- Any report that ignores a given filter today (list, so the new filters can be applied consistently).

PHASE 2 – PLAN (smallest generic change)
- Approved: one change in the shared status helper + the dropdown; any report not using the helper gets the same predicate. Same Start/End window behaviour as the other statuses.
- CAS Manager / Approving Manager: same pattern as CREDIT REVIEW OFFICER – one shared predicate, applied in every CRM report repository; empty = no filter.
- Layout changes are visual only; no change to filter values or payload names.
- New filters appear in the PDF Applied Report Filters page and the Excel Filters sheet with clear labels.
- Do NOT decide silently – list options with recommendations:
  1. Dropdown source for CAS Manager and Approving Manager, and matching rule (exact name vs normalised name, like the RM/PM Employee ID work).
  2. Reports where these filters do not make sense (if any).
  3. Approved status: reviews later Distributed/Finalized – should they still match "Approved" if the approval date is in range? (Recommend: yes, same as other statuses – based on the date column only.)
- No schema change. No SQL scripts unless needed – ask first.

STOP after Phase 2 and wait for my answers. Report in chat only.
