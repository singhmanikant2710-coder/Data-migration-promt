#263 (follow-up you reported) – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: Deleting a CRM finding (or an account in Transactions) refreshes the review. After the refresh, the Scorecard comments – and several other tabs – showed the last saved values instead of your unsaved edits. The edits were not lost, which is why clicking Save brought them back.

What changed: After deleting a CRM finding or an account, all unsaved edits stay visible on every tab (Scorecards, Key Risks, Risk Rating Justification, Regulatory Tracking, Repayment, Collateral, Checklist, Covenants, Policy Exceptions, Transactions and CRM Ratings). Saving works exactly as before.

Tested in Dev: your scenario (Scorecard comments, then deleting a finding), unsaved edits on several tabs followed by a finding delete and an account delete, then Save.

I will confirm here once deployed to QA.

git checkout -- frontend/next-env.d.ts frontend/tsconfig.tsbuildinfo
git add -u frontend
git add "frontend/src/app/review/[ecif]/review-info/stagedOverlay.ts"
git status
git commit -m "UAT #263 follow-up: unsaved edits on Review Form tabs stay visible after deleting a CRM finding or an account (staged changes re-applied on reload)"
git push origin develop
