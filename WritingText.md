#250 – Review Queue: Completed Dates

Status: Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong:
The "Completed" column in the Review Queue was not reading only the CRO Complete date. It showed the first date it found in this order: Mgr Approval, then Finalized, then CRO Complete. Draft Completed reviews looked correct only because they had no approval or finalized date yet. For Approved, Distributed and Finalized reviews, the Mgr Approval date was shown instead.

What changed:
- The "Completed" column now always shows the CRO Complete date, whichever status box is selected (Draft Completed, Approved, Distributed, Finalized or All).
- The Approved, Distributed and Finalized columns were already correct and are unchanged.
- Sorting and searching on the Completed column now use the CRO Complete date as well.

Verified in Dev:
- Review 21727 (Approved): Completed now shows 6/29/2026 (CRO Complete) instead of 7/31/2026 (Mgr Approval).
- Review 21640: Completed now shows 6/12/2026 (CRO Complete) instead of 7/31/2026 (Mgr Approval); Approved, Distributed and Finalized show their own dates.

Please note:
- Status box counts, Review Progress and Review History are not affected. Review Progress intentionally shows a different date for each status bucket, with a matching column header.
- If a review has no CRO Complete date, the Completed column now shows blank instead of borrowing another date.
- The planned Review Queue Excel export (#237) will use the corrected Completed date.

Next step: I will confirm here once this is deployed to QA for your testing.
