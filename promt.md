Subject: CRM PD Grade Migration — approval-date requirement (status-driven reporting)
Hi Geoff,
I looked into your note that the report requires [Review_approval_date] to be populated, which overrides the Reports Home status selection. You're exactly right — I confirmed it in the code.
Currently the report has a built-in "Approved Only" condition that defaults to on, so it only returns reviews where Review_approval_date is populated. The report also doesn't accept the Reports Home "Status" selection at all right now, so that filter has no effect here.
To make it status-driven (so the Reports Home status — Completed / Distributed / Finalized — determines which reviews appear, independent of approval date), I'll wire the report to use the same status logic the Review Status screen uses, and remove the automatic "Approved Only" requirement.
Before I make that change, one confirmation:
The "Approved Only" default appears intentional in the original design — it limited the PD grade migration analysis to reviews that had been approved. Removing it means the report will include reviews at whatever status you select, including those not yet approved. Is that the intended behavior? In other words, should PD grade migration now reflect all reviews matching the selected status, regardless of whether they've been approved?
If yes, I'll proceed with the status-driven change. If there's a case where "Approved Only" should still be available (e.g., as an optional toggle), let me know and I'll keep it as an option rather than removing it entirely.
Everything else you flagged on this report (the color coding, hiding $0 / defaulting to 0, the direction logic, the % columns, and label edits) is done and ready for you to review.
Thanks!
Manikant
