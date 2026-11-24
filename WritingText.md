#258 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: The customer grid shows 25 rows per page, and Load Samples only loaded the rows on the page being viewed. The remaining uploaded selections were then discarded without any message. Loading from page 2, or with a grid filter applied, had the same effect.

What changed:
- Load Samples now loads every selection in the sample that passed validation, regardless of the page shown or any grid filter. The confirmation now says how many will be loaded (e.g. "30 of 30 selection(s) passed validation and will be loaded. Grid filters do not limit the load.").
- The maximum is 75 selections per upload/load. A file with more than 75 is blocked with a clear message ("This file has N selections; the maximum per upload is 75. Please split the file.") and nothing is loaded.
- The grid still shows 25 rows per page.
- The existing duplicate checks (a customer already loaded in the sample is put on hold) are unchanged.

Tested in Dev: 30 selections loaded from page 2 (all 30 reviews created), 75 selections loaded (all 75 reviews created), and a 76-selection file blocked.

Note: a 75-selection load took about 1.5 minutes in Dev, so larger loads may take a minute or more to complete. Please keep the page open until the confirmation appears.

I will confirm here once deployed to QA.


git checkout -- frontend/next-env.d.ts frontend/tsconfig.tsbuildinfo
git add backend/src/Casrr.Application/SampleLoad/ISampleLoadService.cs frontend/src/app/load-samples/page.tsx
git status
git commit -m "UAT #258: Load Samples loads every passing selection of the sample (not just the visible 25-row page); max 75 selections per upload/load with a clear message"
git push origin develop
