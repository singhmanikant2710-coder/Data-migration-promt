The read path works (saved scorecard comments now display correctly on load). But saving a scorecard-comment-only change still returns 400 "No changes were provided".

Root cause: in backend/src/Casrr.Api/Controllers/ReviewController.cs there is a guard that counts how many sections were supplied and returns 400 if none. That guard does NOT check dto.Scorecard, so a scorecard-only save is rejected.

Show me that guard verbatim (read-only first) — the code that builds `postedSections` / counts sections and returns the "No changes were provided" 400.

Then fix it: add dto.Scorecard to that guard exactly the way dto.RiskRatingJustification is handled there, so a scorecard-only save is accepted.

Confirm the guard now recognises Scorecard, and that no other section's behaviour changes.

Show me the diff. STOP after applying.
