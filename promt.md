Fix #180(d): Reconsideration & Appeal unlock — unlock review + hide Unlock button

- Reconsideration/Appeal now call setIsApprovedFromQueue(false) + reloadPageData
  after save (matching General Revisions), so the review reflects unlocked state
  and the Unlock button hides immediately.
- Corrected the initialApproval spread condition to match General Revisions
  (set initial approval on first unlock only).

Verified via SQL + UI for both paths: approval_date & finalized_date cleared,
initial_approval set, Locked=0, Unlock button hidden, sub-form fields saved.
