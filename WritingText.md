Hi Geoff,

I have completed the analysis for #243 (PD/LGD Grade Change Reconciliation) using the Dev data. Before I build it, I need your decisions on a few business rules, because they change the results significantly. For each one I have added my recommendation – a simple "agree" is enough where you are happy with it.

How the report would work (version 1):
- The CAS Admin runs it after each monthly Data Mart upload. It compares, account by account, the CAS PD/LGD grades from reviews finalized in the chosen month against the grades in the latest Data Mart.
- Results are grouped into Reconciled, Not yet updated, and Needs review (with the reason), with RM/PM, review, customer and account details, and the related SM-102/103/104 findings and scorecard assessment shown as reasons.
- Important: the Data Mart only holds one month at a time, so each month's report must be run (and the Excel saved) before the next upload. It cannot be re-created afterwards.

Questions:
1. Which reviews are in scope? Recommendation: reviews FINALIZED in the month. (Using CRO Complete instead gives different results, e.g. in Dev, June: 8 PD differences by Completed date, 0 by Finalized date.)
2. Which month? Recommendation: reviews finalized in the month BEFORE the Data Mart month, so the bank has had time to update its systems. (Alternative: the same month as the Data Mart.)
3. Baseline "bank grade": reviewers can edit Bank PD/LGD on the Scorecards tab. Recommendation: compare against the original grade loaded from the Data Mart, and flag any account where the reviewer changed the bank grade as "Needs review".
4. Expected change: Recommendation: an account is expected to change when its CAS PD or CAS LGD differs from the bank grade. SM-103/104 findings and scorecard assessments are shown as reasons only, because findings can be cited without a grade change.
5. SM-103/104 findings with no CAS vs bank grade difference: list them separately for information only (not as exceptions)? Recommendation: yes.
6. SM-102 (incorrect scorecard): is a new scorecard ID or a later grade date in the Data Mart acceptable evidence that the scorecard was re-run? If neither, the account goes to "Needs review". Recommendation: yes.
7. If PD is updated but LGD is not (or the reverse): Recommendation: "Not yet updated", showing which one is still outstanding.
8. If the bank grade already matched the CAS grade in the prior month (updated during the review): Recommendation: count as Reconciled, labelled "updated before this month".
9. Accounts no longer in the Data Mart (paid off or renumbered): Recommendation: "Needs review", so the Admin can confirm.
10. LGD grades: the bank data contains LGD 10–14, but the CAS LGD drop-down only goes up to 9. Should 10–14 be valid CAS grades, or should these cases go to "Needs review"?
11. Reviews with an appeal or reconsideration after finalization: Recommendation: include them, flagged as "Needs review".
12. Output and confirmation: Recommendation for version 1 – a PDF plus an Excel export (one sheet per result group). The Admin confirms exceptions in the saved Excel; storing confirmations inside CASRR can be added later if needed.
13. Access: Recommendation – CAS Administrators only.

If it is easier, I am happy to go through these in a short call.

Thanks,
Manikant
