Phase 3 — Bug 228. Frontend: hide mutation controls for non-admins on the 9 Library pages, hide CORE Exports entirely, and lock down /maintenance/cas-users. READ-ONLY confirm first, then implement. Show diffs, do NOT commit.

READ-ONLY first:
1. Confirm how useAuth() / isAdmin is currently accessed in a working example (e.g. AdminLayout or the /admin route guard) — this is the pattern to reuse.
2. On ONE library page (e.g. policy-exceptions/page.tsx) as a reference, confirm the exact locations of the "Add" button, the per-row Edit/Delete actions, and any "Add New Category/Component/etc." inline mutation buttons — the mutation surface identified in the earlier investigation.

THEN implement:
3. On EACH of the 9 library pages (cas-findings, covenants, policy-exceptions, sample-criteria, distribution-parties, help-tips, loan-codes, naics, selections): wrap the "Add" button, the per-row Edit/Delete action cell, and any inline "Add New ..." mutation buttons in a check on isAdmin — hide/disable them for non-admins. The table itself (read/view) stays fully visible and functional for everyone. Mirror the same conditional pattern across all 9 pages for consistency.

4. CORE Exports: 
   a. In Sidebar.tsx, hide the Core Exports nav entry for non-admins (mirror how /admin/* entries are already filtered).
   b. Add a route guard on app/maintenance/core-exports/page.tsx (mirror AdminLayout's redirect-to-/no-permissions pattern) so direct URL access is also blocked for non-admins, not just hidden from nav.

5. Lock down app/maintenance/cas-users/page.tsx the same way — add the same route guard (redirect non-admins to /no-permissions). Do NOT delete the route or its functionality (Admins may still use it, or confirm with me if you'd rather have it removed since /admin/users may be the intended single entry point — flag this as a decision point rather than choosing silently).

Do NOT touch backend (Phase 2 already done). Show every file changed with diffs — there will be many (9 library pages + Sidebar + 2 route guards). Rebuild. Do NOT commit.
