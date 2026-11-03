UAT #232 – Concurrent save protection on the Review Form (optimistic concurrency)

REQUIREMENT
If two users (e.g. reviewer and manager) have the same review open and both save, the second save currently overwrites the first silently. CASRR must detect that the review was changed by someone else after it was opened and block the save with: "This review was updated by another user. Please refresh to see the latest changes before saving."
Approved approach: Option 1 only – a row version check on save (HTTP 409). Edit-mode locking (Option 2) is OUT of scope. Normal day-to-day saving must not change.
Business note: "The app has a check for this, but it currently cannot detect that the review was changed by someone else."

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Find the existing concurrency check mentioned by the business (file:line): what it compares (timestamp? LastModified?) and why it cannot detect another user's change.
- List EVERY write path for a review, frontend and backend, with file:line: main Save (/api/v1/reviews/save, ReviewService.SaveAsync), section saves (Covenants, Policy Exceptions, Findings, CRM Ratings, Checklist, Collateral, Repayment, Scorecards, Key Risks, Regulatory Tracking, Transactions, Risk Rating Justification, etc.), unlock/reconsideration/appeal handlers in ReviewInfoSection.tsx, row deletes (PE/covenant/finding deletes), "Save and leave", "Restore and save", #239/#240 save-and-notify, any autosave.
- For each path: which tables it writes, and whether it updates the dbo.[02_CORE_02_Reviews] row at all. (Key risk: a save that only writes a child table would not change a version on Reviews.)
- Every OTHER writer of dbo.[02_CORE_02_Reviews] outside the Review Form (Load Samples, admin screens, Review Queue inline edits, uploads, jobs, DBA scripts) – these would also change the version and could cause conflicts for open users.
- How the review is loaded (DTO/endpoint) and how the frontend keeps state after a save (does it reload the review? does it get anything back?).
- Whether multi-table saves run in one transaction.
- Current table size/row count of Reviews if visible from scripts, and any existing rowversion/timestamp columns.

PHASE 2 – PLAN (smallest generic change)
- One review-level version: a rowversion column on dbo.[02_CORE_02_Reviews], added by an idempotent script in scripts/sql/ (IF NOT EXISTS ... ALTER TABLE ... ADD), with a short DBA note on lock/runtime impact for QA/Prod.
- Load: return the version with the review. Save: every write path sends the version it loaded; the server checks it and writes ATOMICALLY (UPDATE ... WHERE Review_id = @id AND RowVer = @ver, or check+write inside the same transaction) – no check-then-write race.
- Child-table-only saves must also bump the review version (e.g. touch the Reviews row in the same transaction) so a conflict is detected for any section. Propose the exact mechanism.
- After every successful save, the server returns the NEW version and the frontend stores it, so the same user's next save (any section, any order) never gets a false conflict.
- On conflict: HTTP 409 with a ProblemDetails code (e.g. REVIEW_CONFLICT) and the business message; nothing is written (whole save rolls back). The frontend shows the message, KEEPS the user's unsaved edits on screen (does not discard or overwrite them), and offers Refresh. The #239/#240 email modal must not open on a 409.
- Must stay unchanged: all existing save rules (#212, #233 send-only-edited, #235, #249 server guard, #254), payload fields other than the new version, other screens.
- Do NOT decide these silently – list options with a recommendation for me:
  1. A request with NO version (old cached frontend, other callers): reject, or accept without the check?
  2. Writers outside the Review Form (Load Samples, admin, jobs): should they bump the version (and can they cause conflicts for open users)?
  3. Same user with the review open in two tabs: treat as a conflict (recommended) or not?
  4. Should the 409 message show who last updated and when (only if that data already exists)?
  5. Deployment order: code must not fail if the DBA script has not run yet – how will you guard that?
- No other schema change. SQL only as the idempotent script above; I will hand it to the DBA.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code.

PHASE 4 – VERIFY + REPORT (in this chat, do not write the report to a file)
- Existing check found (file:line) and why it failed; ADDED (file:line); REMOVED/CHANGED (file:line); the SQL script content.
- A table of every write path: sends version? bumps version? returns new version? tested how?
- On-screen before/after for: two users/tabs – A saves, then B saves the same section → B blocked; A saves, B saves a DIFFERENT section → B blocked; same user saves twice in a row; same user saves two different sections one after another; save after Refresh succeeds; unlock/re-approve flows; PE/covenant delete; #239/#240 Yes on a stale review.
- NULL/edge cases: missing version in the request; review deleted/cancelled meanwhile; network error after the server committed (next save must not falsely conflict); concurrent saves at the same millisecond.
- Confirm other screens and writers are unaffected.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-modified/untracked SQL scripts and devin-output/ alone; the new scripts/sql file IS part of this change).
- Do not commit or push.
