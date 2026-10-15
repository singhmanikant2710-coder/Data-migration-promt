Hi Geoff,

Thank you for sharing the enhancement list. I have reviewed all 14 items against the current code. Below is my plan to deliver them within 3 weeks, with a short explanation of what each item involves, why it takes the time it does, and the risks I will be managing.

To let you start testing early, I will deliver in three weekly batches, each deployed to QA at the end of the week.

DELIVERY PLAN
Week 1: #248, #245, #238, #234, #236, #237, #247 (plus a short working session on #243)
Week 2: #232, #246, #239, #240, #242
Week 3: #244, #243

------------------------------------------------------------
WEEK 1
------------------------------------------------------------

#248 – Comment "Cancel" → "Undo" + confirmation on top-banner Cancel (High) – Small
What: The comment box button will read "Undo" on every tab. The top-banner Cancel will show a confirmation modal before discarding unsaved edits.
Why the time: The comment box is a shared component, so one change covers all tabs. The modal needs to detect whether anything is actually unsaved, so it only appears when there is something to lose.
Risk: Low. I will check that the new modal does not interfere with Save, tab switching or the return-to-list button.

#245 – Version label on Home screen (Low) – Small
What: Replace the fixed "v1.0" label with the current release version, read automatically from each release so it never goes out of date.
Risk: Very low.

#238 – Bcc to CRO_email and CRO_manager_email (Moderate) – Small
What: Add a Bcc line to the memo distribution email, filled from CORE_Reviews.
Why the time: Blank or invalid addresses must be skipped so the email still opens correctly, and duplicates removed if the same person appears in more than one line.
Risk: Low.

#234 and #236 – Type-to-search for Covenant Type and Policy Exception Description (Low) – Small
What: One shared searchable dropdown used in both places (Performance and Monitoring Covenants, and the 52 Policy Exception options).
Why the time: These dropdowns are inside editable table rows, so the search must work with the row's save logic and keep existing saved values displaying correctly.
Risk: Low. I will confirm that existing saved selections still show and save exactly as before.

#237 – Excel export on Review Queue (Moderate) – Small/Medium
What: An "Excel Export" button that exports what the user currently sees, respecting the same filters and My View.
Why the time: The queue shows data page by page, so the export must fetch the complete filtered list, not just the visible page, and keep values like dates, amounts and IDs correctly formatted in Excel (for example, leading zeros not dropped).
Risk: Low. Very large exports will be tested for speed.

#247 – Special Assets Managed with customer/account sync (High) – Medium
What: Add the Yes/No field to Relationship Overview and keep CORE_Reviews and CORE_Accounts in step in both directions. I will also check which field the Reports Home "Special Assets" filter uses and point it to the customer level if needed.
Why the time: The customer-level and account-level values must be updated in a single database transaction, so a failed save can never leave them out of step. It also has to fit with the save protections we recently added, so one section's save never overwrites another's data.
Risk: Medium, because it changes stored data across two tables and affects reporting. I will test single-account, multi-account and mixed-value reviews.
Questions:
- When an account is changed from Yes back to No, what should happen to the customer level and the other accounts?
- For existing reviews where account values are mixed today, should we leave them as they are, or run a one-time alignment script that you review before the DBA team runs it?

------------------------------------------------------------
WEEK 2
------------------------------------------------------------

#232 – Two users saving the same review (Moderate) – Medium/Large
What: When a second user saves a review that someone else has updated since they opened it, they will see "This review was updated by another user. Please refresh to see the latest changes before saving." instead of overwriting.
Why the time: This needs a new version column on the Reviews table (a small DBA script), plus backend changes so that every section save is checked, including child tables like Covenants, Policy Exceptions and CRM Findings, not only the main review record. Each save path must be updated consistently.
Risk: Medium. If any save path is missed, that section would remain unprotected, so I will test each tab with two users. I recommend this option over the edit lock, because it protects the data in every situation, including when a lock would have expired. A "currently being edited by <User Name>" notice can be added later if still useful.

#246 – 15- and 5-minute warnings before the 4-hour closure (Moderate) – Small/Medium
What: On-screen warnings at 15 and 5 minutes with a "Save now" option.
Why the time: I first need to confirm what triggers the 4-hour closure. If it is our application's session, the warnings and timing are fully in our control. If it comes from the bank's sign-in policy, we can still show the warnings, but extending the session may not be possible from within CASRR.
Risk: Low to medium, depending on that finding. I will confirm it in week 1 and let you know.

#239 and #240 – CRO Complete and Mgr Approval email workflows (Low) – Medium
What: When CRO Complete or Mgr Approval is first set, a Yes/No modal offers to save and notify. On Yes, the review is saved, then the existing email window opens with the To, Subject and Body you described.
Why the time: Both share one modal and reuse the existing email window. The care is in the sequence: the email must only open after the save succeeds, and the "ask again on the next Save, stop after a second No" rule must be tracked correctly.
Risk: Low to medium.
Questions:
- Should the "ask again" rule apply only while the review stays open, or also after closing and reopening it?
- If CRO_manager_email or CRO_email is blank, should the email window still open with an empty To line, or show a message?

#242 – KRI tolerance formatting in the CRM Findings Summary Table (Low) – Small/Medium
What: Show each CRM component's Finding and Unsatisfactory rates against its tolerance, in bold red when breached. Tolerances will be stored in the Selections Library (small DBA script).
Why the time: The rates and tolerances must use exactly the same calculation and rounding as the report, so a value right at the limit is not shown incorrectly.
Risk: Low.
Question: Is a breach when the rate is above the tolerance, or at/above it? And could you share the tolerance values (or placeholders until 2027)?

------------------------------------------------------------
WEEK 3
------------------------------------------------------------

#244 – 7-year record retention (High) – Large
What: An Admin screen to delete Samples, and all their related records, where every review's finalized date is outside the 7-year policy.
Why the time: This is the highest-risk item because deletion is permanent. To do it safely:
- Map every downstream table linked to a Sample (reviews, accounts, covenants, policy exceptions, CRM findings, checklist, transactions, scorecards and others), so no orphaned records are left behind.
- A preview step showing the impacted Sample names and review counts before anything is deleted.
- "Confirm Deletion" runs everything in one database transaction. If any part fails, nothing is deleted, and the screen explains the cause and how to fix it.
- An audit log of who deleted what and when.
- Admin-only access, and full testing on a copy of the data before QA.
Risk: High if rushed, which is why it has a full week. I also recommend the DBA team takes a backup before each annual run.
Questions:
- If a Sample has any review with no Review_finalized_date (never finalized), should that Sample be kept? My suggestion is to keep it and list it as "not eligible", with the reason.
- Should the 7 years be measured from today back to each review's finalized date?

#243 – Monthly PD/LGD grade change reconciliation report (Low) – Large
What: A report on Reports Home that compares the prior month's review results with the latest Data Mart upload and classifies each case as Reconciled, Not yet updated or Needs review, with RM/PM, review and account details.
Why the time: The comparison combines several sources: CAS vs Bank PD/LGD, Scorecard Assessment selections, the SM-102/103/104 findings, and current vs prior month Data Mart grades. Each outcome needs an exact rule, for example how to match loans across months and how to treat accounts that were closed, renewed or re-numbered.
Risk: Medium. The main risk is the rules, not the code. That is why I would like a short working session in week 1, with a few real examples of each outcome, so the report matches how your team reconciles today.

------------------------------------------------------------
WHAT THE 3-WEEK TIMELINE DEPENDS ON
------------------------------------------------------------
- Answers to the questions above during week 1, especially for #247, #244 and #243.
- The DBA team running the small scripts for #232 and #242 (and the selection rows for #242) in QA when they are ready.
- The #243 working session happening in week 1.
- No major new issues arising during UAT of the earlier batches.

If any of these slip, I will let you know straight away with the impact on the dates. I am also happy to change the order if the business needs a particular item sooner.

Thank you,
Manikant
