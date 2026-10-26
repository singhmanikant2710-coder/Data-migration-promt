Hi John, Ashok,

For CASRR UAT #244 (Record Retention), Geoff has asked me to reach out to you on two points:

1. Backup / restore: the new Admin "Record Retention" process permanently deletes Samples (and their Reviews, Accounts, Covenants, Policy Exceptions, Findings, Checklists) whose Sample Closed date is older than 7 years. Before the first QA and Prod runs, can you confirm a backup / point-in-time restore is available and the retention window it covers?

2. Schema change: to keep an audit trail, I plan to add two new audit tables (no changes to existing tables):
   - dbo.[05_AUDIT_02_Record Retention Runs] – who ran it, when, cutoff date, outcome, counts
   - dbo.[05_AUDIT_03_Record Retention Samples] – Sample IDs/names and review counts deleted per run
   They will be delivered as idempotent scripts in scripts/sql/ (with a rollback script) for you to run in QA and Prod. Please let me know what approval process this needs on your side. The app service account will also need DELETE permission on the CORE tables listed above – please confirm it has it.

I'll share the scripts once the design is final.

Thanks,
Manikant


Hi Geoff,

Thanks – agreed, basing it on Closed Samples is simpler and safer. The rule will be: Sample is Closed AND Sample Closed date is before TODAY minus 7 years. The cutoff is calculated automatically (no date entry) and shown on screen, with a preview of the Samples and review counts before you confirm.

As a safety net, any Closed sample that has no Closed date, or that still has a review that is neither Finalized nor Cancelled (possible in older migrated data), will be excluded and shown in the preview with the reason.

On #5: yes, it adds two new audit tables, which Ashok/John will need to create via script – I've reached out to them, along with the backup question (#8). You will see the audit on the same Record Retention screen as a read-only "Run History" list (date, run by, cutoff, outcome, samples and reviews deleted).

Thanks,
Manikant


UPDATE for #244 – business has changed the eligibility rule. Do NOT implement yet.

NEW RULE (from Geoff):
- A Sample is eligible when it is Closed AND its Sample Closed date is before DATEADD(year,-7, CAST(GETDATE() AS date)) (strict <, date-only).
- No Admin date input. The cutoff is computed on the server and shown read-only on the screen.
- Business states a Sample can only be closed when all its reviews are Finalized or Cancelled.

Decisions now fixed:
- a: no date input (auto cutoff). a2: strict < (on-cutoff retained).
- b/c/g: replaced by the Closed + Closed date rule, plus the defensive guard below.
- d: warning only (last successful run date, from audit).
- e: approved – 2 audit tables via idempotent script for DBA, no customer names. ADD a read-only "Run History" grid on the Record Retention page (newest first: run date, run by, cutoff, outcome, samples deleted, reviews deleted). Read-only GET endpoint on the same new controller, RequireAdmin.
- f, i, k, l: approved as you recommended. j: cap 100. m: typed DELETE.

PHASE 1 ADDENDUM – READ-ONLY (no edits):
1. Which column on dbo.[02_CORE_01_Samples] holds the Sample Closed date? Exact name, data type, nullability (from discovery/backend-schema and code). file:line where it is set.
2. Trace the Close Sample flow end to end (UI -> API -> service -> repository -> SQL) with file:line. Is "all reviews Finalized or Cancelled" enforced SERVER-side? Quote the check, or state clearly that it is missing.
3. Can a Sample be reopened (Closed set back to 0)? If yes, file:line, and what happens to the Closed date.
4. Any other place that writes Closed or the Closed date (data load, admin, scripts/sql/).
5. Replace Q6 with a new read-only query using the new rule:
   - cutoff_date
   - samples eligible (Closed = 1 AND closed date < cutoff)
   - of those: how many have at least one review with Review_finalized_date IS NULL AND ISNULL(Cancelled,0) = 0 (data inconsistency)
   - closed samples with NULL closed date
   - closed samples with closed date >= cutoff
   - open samples (Closed = 0 or NULL)
   - eligible samples with pending rows in dbo.[04_TEMP_01_Sample Load]
   - retained reviews whose Prior_CAS_review_id points to a review in an eligible sample
   - eligible sample count and review count (to compare with business expectation of <= 50 samples per run)
   Use ISNULL(Cancelled,0) consistently (your previous Q6 used ISNULL(Cancelled,1) in one count).

PHASE 2 – REVISED PLAN (then STOP):
- Eligibility SQL with the new rule plus a DEFENSIVE GUARD: exclude any eligible Sample that has a review with Review_finalized_date IS NULL AND ISNULL(Cancelled,0) = 0, reason "Data inconsistency: review {id} is neither finalized nor cancelled – contact support". Also exclude Closed samples with NULL closed date, reason "No closed date recorded – contact DBA". Show both in the Excluded list.
- Keep everything else from your plan (server re-eligibility on execute, applock, 409 on preview mismatch, child-first delete order from Q1, count guard, audit inside transaction, safe ProblemDetails with correctiveActions, cap 100, typed DELETE).
- Updated file list and the audit + Run History design.
- Remove the date input from UI, contracts and validation.

Reply in chat only. Do not create any files. Do not commit or push.
