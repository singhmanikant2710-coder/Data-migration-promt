Phase 2 — Bug 228. Apply the RequireAdmin policy (built in Phase 1) to enforce write-restrictions and export-restrictions at the backend. READ-ONLY confirm first, then implement. Show diffs, do NOT commit.

READ-ONLY first:
1. List every write action (POST/PUT/DELETE) across the 9 library controllers: FindingsController, CovenantsController, PolicyExceptionsController, SampleCriteriaController, DistributionPartiesController, HelpTipsController, LoanCodesController, NaicsController, SelectionsController. Confirm each currently only has [Authorize(Policy = "RequireActiveUser")] at the class or method level.
2. Confirm ExportsController's current authorization attribute.
3. Confirm AdminController and CasUsersController's current authorization — including the 3 [AllowAnonymous] endpoints in AdminController (lines 209, 244, 279) and what they do.

THEN implement:
4. For each of the 9 library controllers: add [Authorize(Policy = "RequireAdmin")] on each POST/PUT/DELETE action specifically (method-level, not class-level — GET actions must remain accessible to all active users for read access).
5. ExportsController: add [Authorize(Policy = "RequireAdmin")] at the CLASS level (the whole controller is export-only, no read/write split needed).
6. AdminController and CasUsersController: add [Authorize(Policy = "RequireAdmin")] at the class level. Remove [AllowAnonymous] from the 3 endpoints that have it (confirm what those 3 endpoints actually do first, in case any are meant to be public for a legitimate reason like initial setup — flag if so rather than blindly removing).

Do NOT change frontend yet. Do NOT change GET/read endpoints on library controllers. Show every file changed with diffs. Rebuild. Do NOT commit.
