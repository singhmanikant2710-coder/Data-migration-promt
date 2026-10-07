Subject: CASRR – Proposed enhancement: protection against simultaneous edits on a review

Hi Geoff,

While fixing the rating issue, we identified an area where CASRR can be made more robust, and I'd like to propose it as an enhancement.

Current behaviour
If two users (for example, a reviewer and a manager) have the same review open at the same time and both save changes to the same section, the second save overwrites the first one without any warning. The app has a check for this, but it currently cannot detect that the review was changed by someone else in the meantime.
The rating fix we just delivered already reduces the impact – a save now only updates the fields that were actually changed – but if both users edit the same field, the earlier change can still be lost silently.

Proposed enhancement
- CASRR will detect when a review has been updated by another user after it was opened.
- Instead of overwriting, the second user will see a clear message, e.g. "This review was updated by another user. Please refresh to see the latest changes before saving."
- No changes to how reviewers work day to day; they will only see the message in this specific situation.

Benefits
- Prevents silent loss of reviewer/manager work.
- Gives reviewers confidence that what they save is what is stored.
- Supports data integrity and audit expectations for the review records.

What is needed
1. Database: one small change to the Reviews table – adding a version column that SQL Server updates automatically on every save. This will be provided as a reviewed script (with a rollback script) for the DB team (John/Ashok) to run in QA and then Prod.
2. Application: backend and frontend changes to check the version on save and show the message.
3. Testing: verification in QA, including two users editing the same review at the same time, before release to Prod.
4. Approval: your go-ahead to proceed, and a suitable deployment window with the DB team.

No existing data is changed by this enhancement, and it does not affect reports or PDFs.

Separately, we also found two sections (Covenants and Policy Exceptions) that use the same save pattern as the rating issue. They are not causing any problem today, but I'd like to apply the same safeguard there as a small preventive fix – no database change is needed for that.

Please let me know if you'd like to proceed, and I'm happy to walk you through it on a quick call.

Thanks,
Manikant
