1: (a) distinct values from Reviews (CRO_manager_name / Review_approver_name), exact trimmed match, cancelled reviews not excluded from the lists. Do not normalise names.
2: Agreed – no change to Initial/Final Memo or CRM Final Memos; PD Grade Migration gets Approved only, not the new manager filters.
3: Yes – Approved is based on Review_approval_date only (set and within the window), same as the other statuses.
4: Yes – add small tests for Approved in ReviewStatusHelperTests and ReviewStatusEnumTests only.
Proceed with Phase 3 and 4. In the report include: ADDED/CHANGED (file:line), on-screen before/after of the filter layout, one read-only SQL per new filter to compare counts with a report, dotnet build + tests, npx tsc --noEmit, npm run build, git status. Do not commit or push.
