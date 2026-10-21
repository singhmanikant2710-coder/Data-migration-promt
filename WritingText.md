UAT #249 – Review History: orphaned records from unlocked Finalized reviews in Closed samples.

Background: Review History shows only Finalized reviews from Closed samples. An Admin can unlock a review in a Closed sample (this clears Review_finalized_date), edit it, and re-approve it without entering a new Finalized date or a new Mgr Approval date. The review then drops out of Review History and downstream reports (orphaned).

Geoff's options:
1. Block unlocking a Finalized review in a Closed sample until the sample is re-opened (the existing sample-closure check then confirms all reviews are Cancelled or Finalized before it can close again).
2. Block saving an unlocked Finalized review in a Closed sample unless a new Mgr Approval date AND a new Finalized date are entered.
3. When unlocking, keep the Distributed and Finalized dates locked (original Finalized date kept) and require the new Mgr Approval date to be on or before that Finalized date.

THIS ROUND IS READ-ONLY. Do not change any code.

PHASE 1 – Discovery (file:line):
1. Unlock: where is the Admin unlock (UI + API + SQL)? Exactly which columns does it clear or change (Review_finalized_date, approval, distributed, status, lock flags)? Is it audited?
2. Sample open/close: how is a sample closed and re-opened? What does the closure check validate? Who can re-open?
3. Re-approval after unlock: which fields can the user edit, and how do the #212 date-lock rules (Distributed/Finalized locked until Mgr Approval set, IsDateChange) behave for an unlocked review?
4. Review History: which SQL defines what appears (SqlReviewHistoryRepository.cs ~102)? Which other reports/screens filter on Review_finalized_date or sample Closed?
5. Data check – give read-only SQL in the report (do not save it as a script): reviews in Closed samples that are neither Finalized nor Cancelled today (current orphans), with Review_id, sample, status and dates.

PHASE 2 – Options analysis (no implementation):
For each of Geoff's 3 options give: what changes (frontend/backend/SQL, file:line), whether a schema change is needed, how it is enforced server-side (not only in the UI), what the Admin and reviewer would see, effort (small/medium/large), and risks (e.g. re-opening a sample affecting other screens, existing orphans, the #244 retention work).
Then recommend one option with a short reason. Also say how existing orphans should be handled (report only / one-time script for DBA review).

Report only. Do not change code, do not commit or push.
