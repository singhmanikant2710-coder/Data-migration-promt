UAT #243 – PD/LGD Grade Change Reconciliation report (DISCOVERY + RULES PROPOSAL ONLY – do not implement)

REQUIREMENT (Geoff)
A new report on the Reports Home page, run by the CAS Administrator after each monthly Data Mart upload. It compares the PRIOR month's review results with the latest loan data to confirm that PD/LGD grade changes (and scorecards) identified by CAS were updated in the bank's core systems. Inputs:
- CAS PD/LGD vs Bank PD/LGD (review/account level),
- Scorecard Assessment selections,
- Scorecard Management findings SM-102, SM-103, SM-104,
- the monthly Data Mart upload, which has current PD/LGD grades AND Prior Month PD/LGD grades.
Output buckets:
- Reconciled – the loan system now shows the expected PD/LGD change;
- Not yet updated – expected change still missing (with RM/PM, review and account details);
- Needs review – cases that cannot be reconciled automatically.
The CAS Admin then confirms the exceptions instead of checking every borrower manually ("first tier of reconciliation").

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Data Mart: dbo.[01_DATA_01_Data Mart Trial] – exact columns for current and Prior Month PD/LGD (Bank PD, Bank LGD, prior-month equivalents), the account/facility/obligor keys, the as-of/month date column, RM/PM columns. Is the table truncated and reloaded each month (only one month present at a time)?
- Accounts: dbo.[02_CORE_04_Accounts] – Bank_PD, CAS_PD, Bank_LGD/CAS_LGD (exact names), account keys, how they map to Data Mart rows.
- Reviews: which date defines "prior month's review results" (Completed, Mgr Approval, Distributed, Finalized?) – list the candidates and what each means in the app.
- Scorecard Assessment: tables/columns behind the Scorecards tab (selections, PD/LGD scorecard values) with file:line.
- Findings: how SM-102, SM-103, SM-104 are stored in dbo.[02_CORE_07_Findings] (codes, level, link to review/account), and their descriptions from the CAS Findings library.
- Existing report framework: how a new report is added (Selections entry in 03_LIBRARY_09_Selections, ReportsController, repository, PDF component, routing guard, filters, Excel export if any). Use an existing similar report as the template and name it.
- Run a read-only SQL in Dev (give it to me) that shows, for last month's reviews, the counts of accounts where CAS PD/LGD ≠ Bank PD/LGD, and how many of those already match the current Data Mart grade.

PHASE 2 – RULES PROPOSAL (no code)
Write a clear proposal in chat:
1. Which reviews are in scope ("prior month" definition) and which accounts.
2. What counts as an "expected change" for PD and for LGD (CAS ≠ Bank at review; SM-102/103/104 finding present; scorecard selections) – one rule per source, with how they combine.
3. Matching key between review accounts and Data Mart rows, and what happens when an account is missing, paid off, or appears twice.
4. Bucket rules with examples: Reconciled, Not yet updated, Needs review (list every Needs-review case: no match, partial change, changed to a different grade than CAS, PD changed but LGD not, etc.).
5. Role of the Data Mart "Prior Month" grades (e.g. confirm the change happened this month vs earlier).
6. Report layout: PDF following the existing report conventions (header/footer, Applied Report Filters), columns per bucket (RM/PM, review, customer, account, CAS vs Bank vs Data Mart grades, finding codes), plus whether an Excel export is needed.
7. "CAS Admin confirms exceptions": option A = report only (no storage, version 1), option B = store confirmations (needs a new table + DBA script). Recommend one; do not assume B.
8. Access: Admin only? Filters needed (sample, month, segment, RM/PM)?
9. A numbered list of open QUESTIONS for Geoff wherever the data or the requirement does not decide the rule.
10. Effort/risk estimate for version 1.

STOP after Phase 2. Do not implement, do not create SQL scripts, do not change files. Report in chat only.
