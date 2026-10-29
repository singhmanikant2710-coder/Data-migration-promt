#244 Record Retention – Q1–Q6b and N1–N4 results from DEV. PHASE 3 APPROVED.

DB FINDINGS (DEV):
- All FKs are ON DELETE CASCADE: Reviews.Sample_id -> Samples; Accounts/Covenants/Policy Exceptions/Findings/Checklists.Review_id -> Reviews; 04_TEMP_01_Sample Load.Sample_id -> Samples. 6 of 7 FKs are is_not_trusted = 1. Keep the EXPLICIT child-first deletes anyway (accurate per-table audit counts, and works even if Prod FKs differ). Capture per-table counts BEFORE deleting.
- No self-FK on Reviews.Prior_CAS_review_id – leave retained reviews untouched; warning count only.
- 02_CORE_03_Scorecards does NOT exist – remove it from code, contracts, audit table (drop Scorecards_deleted column) and delete order.
- No FK from Sample Load to Data Mart. 04_TEMP_02 has no Sample_id – not touched.
- Triggers: only INSERT/UPDATE leading-zero triggers on Reviews – none on DELETE.
- No temporal/CDC/change tracking/replication.
- Child tables have NO Sample_id column – delete by Review_id only (as planned).
- No index on Review_id for Accounts and Covenants – do NOT add indexes (schema change); volumes are small and capped.
- Samples.Closed is a NULLABLE bit in SQL – use [Closed] = 1 for eligibility and ISNULL([Closed],0) where checking open.
- Closed_Date, Cancelled_date, Review_finalized_date are datetime2; Sample_type nvarchar, Sample_year smallint – all exist.
- N1 on DEV: 39 closed samples, all with Closed_Date >= cutoff, 0 NULL closed dates, 0 eligible.

FINAL DELETE ORDER (inside one transaction): Checklists -> Findings -> Policy Exceptions -> Covenants -> Accounts (by Review_id in #rr_reviews) -> Reviews -> 04_TEMP_01_Sample Load (by Sample_id) -> Samples.

IMPLEMENT the approved revised Phase 2 plan with guards A (review neither finalized nor cancelled), B (no closed date) and C (review activity on/after cutoff), cap 100, typed DELETE, Run History, quarterly warning, auth guard, safe ProblemDetails with correctiveActions. Touch only the files in your approved list (+2 DI lines, +1 Sidebar line). SampleController, SqlSampleRepository, SampleLoad code, AdminController, Dialog.tsx and all shared helpers stay untouched.

SQL scripts (scripts/sql/, idempotent, with rollback script and a verification SELECT at the end):
- create-record-retention-objects.sql / rollback-create-record-retention-objects.sql – audit tables only. No other schema changes.

TEST DATA (DEV ONLY – put it in test-data/, never commit):
- A seed script that INSERTS new samples named 'RR_TEST_...' (does not modify any existing sample) with reviews and at least one row in each child table, covering:
  1) ELIGIBLE: Closed = 1, Closed_Date 8 years ago, all reviews finalized/cancelled more than 7 years ago.
  2) Guard A: same, but one review neither finalized nor cancelled.
  3) Guard B: Closed = 1, Closed_Date NULL.
  4) Guard C: Closed_Date 8 years ago but one review finalized 2 years ago.
  5) Not eligible: Closed_Date 3 years ago.
  6) Open sample (Closed = 0) with an old date.
  7) ELIGIBLE with zero reviews.
- A read-only verification query showing per-table row counts for the RR_TEST_ samples (before/after).
- A cleanup script that removes any remaining RR_TEST_ rows.

PHASE 4 – VERIFY + REPORT (in chat):
- Root cause / what was needed; ADDED (file:line); REMOVED/CHANGED (file:line).
- On-screen before/after table (new Admin > Record Retention page, Sidebar item visible only to admins).
- NULL/edge cases: each of the 7 test samples (expected status + reason), date exactly on cutoff, non-admin calling the APIs directly (403), wrong confirmation text, more than 100 ids, eligibility changed between preview and execute (409), audit tables missing (Execute disabled + message), mid-transaction failure (rollback proven, Failed run row written).
- Regression checklist: Samples / Load Samples page, Review Queue, Review Form, Review History, Reports + PDFs, DP Upload, other Admin pages – unaffected and how verified.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status: restore next-env.d.ts and tsconfig.tsbuildinfo if rewritten; leave untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone; test-data/ and devin-output/ must not be staged.
- Do not commit or push.


Hi Ashok, Geoff, team,

For CASRR UAT #244 (Record Retention), the design is agreed with Geoff: a Sample can be purged when it is Closed AND its Sample Closed date is older than TODAY minus 7 years (today's cutoff: 29 Oct 2019). The Admin sees a preview first and must confirm; every run is audited.

Before we release it, we need to understand the PROD data, because our DEV database only has recent samples (nothing older than 7 years, so nothing is eligible there).

Why this matters (Geoff): older Samples migrated from Access may have no Sample Closed date recorded, or may have been given a recent date during migration. In either case the purge would find nothing to delete until that is corrected. These queries tell us if that is the case.

@Ashok – could you please run the 4 queries below in PROD and share the results? They are SELECT-only (no changes to data).

-- 1) Closed Samples by Closed-date year (and how many have no date)
SELECT YEAR([Closed_Date]) AS closed_year,
       COUNT(*) AS samples,
       SUM(CASE WHEN [Closed_Date] IS NULL THEN 1 ELSE 0 END) AS no_closed_date
FROM dbo.[02_CORE_01_Samples]
WHERE [Closed] = 1
GROUP BY YEAR([Closed_Date])
ORDER BY closed_year;

-- 2) Summary against today's 7-year cutoff
DECLARE @cutoff date = DATEADD(year, -7, CAST(GETDATE() AS date));
SELECT @cutoff AS cutoff_date,
       COUNT(*) AS total_samples,
       SUM(CASE WHEN ISNULL([Closed],0) = 0 THEN 1 ELSE 0 END) AS open_samples,
       SUM(CASE WHEN [Closed] = 1 THEN 1 ELSE 0 END) AS closed_samples,
       SUM(CASE WHEN [Closed] = 1 AND [Closed_Date] IS NULL THEN 1 ELSE 0 END) AS closed_no_date,
       SUM(CASE WHEN [Closed] = 1 AND CAST([Closed_Date] AS date) < @cutoff THEN 1 ELSE 0 END) AS closed_before_cutoff
FROM dbo.[02_CORE_01_Samples];

-- 3) Reviews by Finalized year (how far back the data goes)
SELECT YEAR([Review_finalized_date]) AS finalized_year, COUNT(*) AS reviews
FROM dbo.[02_CORE_02_Reviews]
GROUP BY YEAR([Review_finalized_date])
ORDER BY finalized_year;

-- 4) Foreign keys on Samples/Reviews (to confirm PROD matches DEV – in DEV all are ON DELETE CASCADE)
SELECT OBJECT_NAME(fk.parent_object_id) AS child_table,
       OBJECT_NAME(fk.referenced_object_id) AS parent_table,
       fk.name AS fk_name,
       fk.delete_referential_action_desc AS on_delete,
       fk.is_disabled, fk.is_not_trusted
FROM sys.foreign_keys fk
WHERE fk.referenced_object_id IN (OBJECT_ID(N'dbo.[02_CORE_01_Samples]'), OBJECT_ID(N'dbo.[02_CORE_02_Reviews]'))
ORDER BY parent_table, child_table;

Two more items for the DBA team (John / Ashok), as mentioned earlier:
- Backup: these deletes are permanent and the CORE tables have no history/CDC tracking, so a backup / point-in-time restore is the only way to recover. Please confirm it is available and its retention window before the first QA and PROD runs.
- Audit tables: I will share an idempotent script (with rollback) to create two new audit tables – [05_AUDIT_02_Record Retention Runs] and [05_AUDIT_03_Record Retention Samples]. No changes to existing tables. Please let me know the approval process on your side, and confirm the app service account has DELETE permission on the CORE tables.

Thanks,
Manikant
