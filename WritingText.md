#262 – Implemented. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What changed (Reports Home):
- Review Status now includes "Approved", in the order Completed > Approved > Distributed > Finalized. Approved uses the Mgr Approval date (with Start/End dates when entered) and applies to all CRM reports. In CRO Review Production the date column reads "APPROVED DATE".
- Special Assets Managed and Centralized Commercial (CCL) moved below Portfolio Industry in Portfolio Filters.
- Credit Review Officer moved to the top of General Filters.
- New "CAS Manager" filter (the CAS manager stored on the review) and new "Approving Manager" filter (the approver name stored on the review), both applied to all CRM reports. They also appear on the Applied Report Filters page and in the Excel Filters sheet.

Notes:
- The CAS Manager and Approving Manager lists show names exactly as stored on the reviews, so some people appear in more than one spelling (e.g. "Kim Doane" / "Kimberly Doane", "RAMYA, KOVI" / "Kovi Ramya"). If you would like these combined, the stored names would need to be standardised (DBA update) – please let me know.
- CRM PD Grade Migration and CRM Final Memos do not use the CAS Manager / Approving Manager filters (as before with Credit Review Officer).

Tested in Dev: new filter layout, Approved status for 2025 (counts matched the database), and the Approving Manager filter (counts matched the database).

I will confirm here once deployed to QA.


git commit -m "UAT #262: Reports Home filters – Approved review status, CAS Manager and Approving Manager filters applied to all CRM reports; Special Assets/CCL and Credit Review Officer moved"
git push origin develop
