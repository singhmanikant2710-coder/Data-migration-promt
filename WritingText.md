TASK: CASRR UAT #244 – Record Retention (Admin Maintenance). Permanently delete Samples (dbo.[02_CORE_01_Samples]) and ALL related downstream rows where ALL dbo.[02_CORE_02_Reviews].Review_finalized_date values fall outside the 7-year retention period. HIGHEST RISK item – permanent deletes. Existing functionality must NOT break.

Required workflow:
- Admin-only screen/button under Maintenance (src/app/admin, gated by app/admin/layout.tsx + RequireAdmin policy; backend MUST also enforce RequireAdmin).
- Admin enters date parameter(s).
- Server validates the date(s) against 7 years from TODAY (cutoff = DATEADD(year,-7, CAST(GETDATE() AS date)); date-only comparison).
- Preview: list impacted Sample Name(s) with review count (and per-table row counts) that will be deleted, plus EXCLUDED samples with the reason.
- 'Confirm Deletion' button.
- Success or Failure message. Failure must give corrective steps the Admin can take (never raw SQL/exception text).

=== PHASE 1 – READ-ONLY DISCOVERY (NO EDITS) ===
1. List EVERY table that references Samples or Reviews: all FOREIGN KEYs (sys.foreign_keys) plus every table with a Sample_id / Review_id (or similar) column (INFORMATION_SCHEMA.COLUMNS), including 02_CORE_04_Accounts, 05_Covenants, 06_Policy Exceptions, 07_Findings, 08_Checklists and any review-history, email, unlock, attachment, audit or temp tables. Give each table, key column, FK yes/no, ON DELETE behaviour, and the required delete order (children first). Provide this as a read-only SQL query I can run; do not assume.
2. Check for triggers on these tables (sys.triggers) that could fire or block on delete.
3. Trace existing Admin patterns end to end with file:line (UI → src/services/api/admin.ts → AdminController.cs → service → repository → SQL), especially the Distribution Parties Upload (preview → confirm → atomic replace → audit row). Reuse that pattern.
4. Confirm how RequireAdmin is applied server-side (file:line).
5. Confirm how errors are returned today (note AdminController.cs:176 returns raw SQL error in ProblemDetails.Detail – the new endpoints must NOT do this).
6. List every caller of anything you plan to change. Expected: NO shared helper/repo changes (SqlReviewRepository.cs, SqlReviewStatusHelper, pageSetup.ts, HtmlRichText.tsx untouched).

=== PHASE 2 – PLAN, THEN STOP AND WAIT FOR MY APPROVAL ===
Propose the smallest additive design: new endpoints (e.g. POST /api/v1/admin/record-retention/preview and /execute), new service + new repository class, new admin page + nav entry. Existing API contracts unchanged.
Mandatory design rules:
- Eligibility computed ONLY on the server, in SQL. Execute must NOT trust a client sample list: it re-runs eligibility with the same cutoff inside the transaction and deletes only Samples that are still eligible (optionally intersected with the previewed Sample IDs). If the eligible set differs from the preview, abort with a clear message asking the Admin to re-run preview.
- One transaction (SERIALIZABLE or appropriate locking), children → parents order, full rollback on any failure. Consider command timeout for large volumes; state your approach.
- Parameterised SQL only. Table names use spaces and brackets.
- Failure messages map known causes to corrective actions (e.g. "Re-run preview", "Sample X has unfinalized review Y", "Contact DBA – related table Z blocked delete"); log details server-side only.
- Frontend checks are UX only; all rules enforced server-side.
DO NOT decide these silently – list options with your recommendation:
 a) Date input: single cutoff date vs From/To range.
 b) Samples with any review where Review_finalized_date IS NULL (incl. Cancelled): exclude (recommended) vs include.
 c) Samples with zero reviews: include or exclude.
 d) Quarterly frequency: hard block (needs last-run record) vs warning only.
 e) Audit: new table (e.g. dbo.[05_AUDIT_02_Record Retention]) – who, when, cutoff, sample IDs/names, per-table row counts, success/failure. This is a schema change – only propose it, as an idempotent script in scripts/sql/ for the DBA; do not create it.
 f) Any table found in discovery whose relationship is unclear (keep vs delete).
STOP after Phase 2 and wait for my reply.

=== PHASE 3 – IMPLEMENT (only after my approval) ===
Touch only the new files plus minimal registration (DI, route, nav entry). Any SQL objects only as idempotent scripts in scripts/sql/ (IF NOT EXISTS). No changes to existing repositories, reports, PDFs or shared helpers.

=== PHASE 4 – VERIFY + REPORT ===
- Root cause / what was needed.
- ADDED (file:line) and REMOVED/CHANGED (file:line).
- On-screen before/after table.
- NULL/edge cases: NULL finalized date, Cancelled reviews, sample with 0 reviews, sample with mixed in/out dates, date exactly on cutoff, future date, date inside 7 years, leap day, empty result, non-admin user calling the API directly (must be 403), data changed between preview and confirm, mid-transaction failure (prove rollback).
- Regression checklist: Admin pages, DP Upload, Review Queue, Review History, Reports – confirm unaffected and how verified.
- Read-only SQL I can run before/after to verify row counts per table for a test Sample.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts and tsconfig.tsbuildinfo if rewritten; leave the untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone).
- Do not commit or push.
