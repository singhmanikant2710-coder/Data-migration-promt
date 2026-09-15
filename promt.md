Bug 228 — Access control for Maintenance screens. READ-ONLY, no edits. One pass, answer everything, STOP.

CONTEXT: Non-Admin CROs currently have full edit access to Library maintenance screens (e.g. Policy Exceptions library, CAS Findings library, Selections, etc.) and can see/use the CORE Exports page. Geoff confirmed Users table access is ALREADY hidden from non-admins — some permission mechanism already exists in the app. Goal:
1. Libraries (maintenance screens): Non-Admin users get READ-ONLY access (can view, cannot edit/add/delete).
2. CORE Exports page: HIDDEN entirely from Non-Admins (not just disabled — not visible/accessible at all, including direct URL access).
3. Admins: unchanged, full access to everything.

Investigate:
1. How does the app currently determine if a user is an "Admin"? Find the role/permission model — is there an "Administrator" flag/role in the Users table (03_LIBRARY_05_CAS_Users or similar)? How is it read (backend claim, JWT, session, API call)?
2. How is the Users table ALREADY hidden from non-admins today? Find the exact mechanism — frontend route guard, sidebar conditional rendering, backend authorization policy (e.g. [Authorize(Policy = "RequireActiveUser")] mentioned in earlier work — is there an Admin-specific policy too?), or something else. This is the pattern to mirror.
3. List every "Maintenance" screen/route currently in the app (Policy Exceptions library, CAS Findings library, Selections, CORE Exports, Users, any others) — file paths for each page/route.
4. For the Library screens specifically: are Add/Edit/Delete actions separate UI components/buttons that could be conditionally hidden/disabled for non-admins, or is the whole page one editable form that would need broader changes to go read-only?
5. For CORE Exports: is it a single page/route that can be gated by a route guard (frontend) + endpoint authorization (backend), mirroring however Users table access is currently blocked?
6. Is the "Admin" check available on both frontend (for hiding UI) AND backend (for blocking the API even if someone bypasses the UI)? Confirm whether backend authorization already exists for the Users-table-hidden pattern, or if that's frontend-only (which would be a security gap worth flagging).

Report the existing role/permission mechanism, the Users-hiding pattern to mirror, every affected Library screen, and whether backend enforcement already exists or needs adding. Do NOT propose or write a fix yet.
