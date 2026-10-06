cd C:\Users\CC438\source\repos\fhn-casrr
git branch

git pull

git rm --cached test-data/rrj-20120.html

git checkout HEAD~1 -- legacy/CAS_RiskReview_v3.accdb

echo test-data/ >> .gitignore
git add .gitignore legacy/CAS_RiskReview_v3.accdb

git status

git commit -m "chore: revert accidental commit of test-data file and legacy accdb"
git push
