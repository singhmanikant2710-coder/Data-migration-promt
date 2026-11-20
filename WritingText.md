Subject: CASRR – Request to create Jira ticket (QA) and CR (PROD) for Record Retention audit tables – UAT #244

Hi Nitika,

Could you please create the following for CASRR UAT enhancement #244 (Record Retention):
1. A Jira ticket for the DBA team to execute the script in QA – today.
2. A Change Request (CR) for PROD – to be executed at the date/time confirmed by Geoff (CAS business owner).

Please use the details below. The script files are attached.

------------------------------------------------------------
1) JIRA TICKET – QA

Title:
CASRR – Create Record Retention audit tables in QA (UAT #244)

Description:
Application: CASRR (Credit Assurance Services RiskReview)
Environment: QA (CAS_RISKREVIEW_TEST)
Requested by: Manikant Singh
Business approver: Geoffrey Houlditch (CAS)
Target date: Today, 20 Nov 2026

Summary:
UAT enhancement #244 adds an Admin "Record Retention" process that permanently deletes closed Samples (and their related records) whose Sample Closed date is older than 7 years, per the FHN/CAS 7-year retention policy. Every run is audited. This ticket requests creation of the two audit tables used by that process.

Script to execute:
scripts/sql/create-record-retention-objects.sql
- Creates 2 NEW tables only: dbo.[05_AUDIT_02_Record Retention Runs] and dbo.[05_AUDIT_03_Record Retention Samples], plus 1 index.
- No changes to existing tables, data, triggers or permissions.
- Idempotent (safe to re-run). Ends with a verification SELECT – expected result: both tables listed.

Rollback (only if required):
scripts/sql/rollback-create-record-retention-objects.sql (dry run by default; will not drop tables containing audit data).

Confirmations required from DBA:
1. The CASRR app service account has DELETE permission on: 02_CORE_01_Samples, 02_CORE_02_Reviews, 02_CORE_04_Accounts, 02_CORE_05_Covenants, 02_CORE_06_Policy Exceptions, 02_CORE_07_Findings, 02_CORE_08_Checklists, 04_TEMP_01_Sample Load; and INSERT/SELECT on the two new audit tables.
2. A backup / point-in-time restore is available for QA.

------------------------------------------------------------
2) CHANGE REQUEST – PROD

Title:
CASRR – Create Record Retention audit tables in PROD (UAT #244)

Description:
Application: CASRR (Credit Assurance Services RiskReview)
Environment: PROD
Requested by: Manikant Singh
Business approver: Geoffrey Houlditch (CAS)
Implementation window: Date/time to be confirmed by Geoff, in the same release window as the CASRR code deployment containing #244.

Change:
Execute scripts/sql/create-record-retention-objects.sql – creates 2 NEW audit tables (dbo.[05_AUDIT_02_Record Retention Runs], dbo.[05_AUDIT_03_Record Retention Samples]) and 1 index. No changes to existing tables or data.

Risk / impact: Low. Additive objects only; no existing functionality affected. The tables are required for the new Admin "Record Retention" screen; without them the screen opens but deletion is disabled.

Pre-requisites:
- Successfully executed and verified in QA (Jira ticket above).
- Confirm the CASRR app service account has DELETE permission on the CORE tables listed above, and INSERT/SELECT on the new audit tables.
- Confirm a backup / point-in-time restore is available, with its retention window, before the first PROD Record Retention run (deletes performed by the process are permanent; these tables have no temporal/CDC/change tracking).

Verification: the verification SELECT at the end of the script lists both new tables.

Back-out plan: scripts/sql/rollback-create-record-retention-objects.sql (drops only the two new audit tables; refuses if they contain data).

Testing: Tested end-to-end in DEV (CAS_RISKREVIEW_DEV) – preview, exclusion rules, full rollback on failure, successful delete with matching per-table counts, and audit rows.
------------------------------------------------------------

Please share the Jira and CR numbers once created. I will confirm the PROD window with Geoff and update the CR.

Thanks,
Manikant
