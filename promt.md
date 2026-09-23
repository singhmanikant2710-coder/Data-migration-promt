Something went wrong — the changes from commit a5b4ff4 (RM/PM canonical name
fix) appear to have been removed/reverted. Before redoing any work,
investigate and report back:

1. Run `git log --oneline -10` on the develop branch — is commit a5b4ff4
   still present in the history?

2. Run `git show a5b4ff4 --stat` — confirm what file(s) that commit touched.

3. Open the actual current content of SqlReviewRepository.cs around the
   ResolveDistributionPartyByEmployeeIdAsync method and the
   @RelationshipManager/@PortfolioManager parameter binding — does the
   canonical-name logic from that commit currently exist in the file, or is
   it gone?

4. Run `git status` and `git diff` — is there an uncommitted change that
   reverted the file back to its pre-a5b4ff4 state? If so, what does the diff
   show, and do you have any idea what would have caused it (a git checkout,
   a reset, a manual edit, a merge, a tool auto-reverting)?

5. Run `git reflog -10` — this shows recent HEAD movements and can reveal if
   a reset/checkout happened after the commit.

Don't re-apply the fix yet — first tell me exactly what state the repo and
file are actually in, and what likely caused the change to disappear, so we
understand the cause before just redoing the work and risking the same thing
happening again.
