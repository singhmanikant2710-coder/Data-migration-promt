Context: CASRR (SQL Server). Production banking app – rollback scripts must never remove data that existed before our forward scripts ran.

TASK: The DBA team asked for rollback scripts for these 5 forward scripts in scripts/sql/:
1. create-distribution-parties-upload-objects.sql
2. insert-crm-findings-for-management-selection.sql
3. insert-crm-summary-for-management-selection.sql
4. insert-help-tip-checklist-questions.sql
5. insert-samples-type-selections.sql

For each, first check whether a rollback section/script already exists. If not, create a SEPARATE file next to it named rollback-<same-name>.sql. Do not modify the forward scripts.

Rules for every rollback script:
- Read the forward script and undo ONLY what it creates/inserts – match rows by their exact natural keys (Tab/Section/Selection value, Help_tip_form + Help_tip_topic, etc.), never by "latest id" or broad filters.
- Default is DRY RUN: DECLARE @Execute bit = 0; with @Execute = 0 it only SELECTs/PRINTs exactly what would be removed (row count + rows). Deletes/drops happen only when @Execute = 1.
- Wrap changes in BEGIN TRY / BEGIN TRAN / COMMIT, with CATCH -> ROLLBACK + THROW.
- Idempotent: safe to run twice (IF EXISTS checks); running when nothing to undo just prints "Nothing to roll back".
- Header comment: what it undoes, prerequisites, risks, and environment notes.
- Ends with a verification SELECT.

Specific safety:
- Distribution Parties objects (04_TEMP_03 staging, 05_AUDIT_01 audit): before dropping, show row counts; if the audit table has rows, refuse to drop unless an extra flag @DropEvenIfHasData = 1 is set (audit history would be lost). Drop in the correct dependency order (constraints/indexes first if needed). Do NOT touch 03_LIBRARY_10_Distribution Parties itself.
- Selections (CRM reports, Samples Type): delete only the exact rows the forward script inserts. If any of those rows look like they pre-existed (e.g. different Selection_id/order than the script would create), list them and skip unless @Execute = 1 and a comment in the header warns about it. Note in the header that Samples Type rollback makes the Samples screen fall back to the built-in list (no break).
- Help tip: header must state clearly "Do NOT run in QA – this row existed in QA before the forward script (forward was a no-op there)". Delete only the row matching form '04_REVIEW FORM_04' + topic 'Checklist Questions'.

Do not run any script against any database.

Report:
1. For each forward script: did a rollback already exist? (yes/no, where)
2. ADDED files (path) with a short summary of what each undoes
3. Safety checks per script (dry-run output, data guards, QA notes)
4. Anything in a forward script that cannot be cleanly rolled back, and why
5. git status (only the new rollback-*.sql files; no harness/temp files)
Do not commit or push.
