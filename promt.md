Bug 198 fix. SINGLE FILE, ONE change. Do NOT touch anything else. Show diff before applying.

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlSampleLoadRepository.cs
The recent-review Hold window regressed. Legacy Access query "01_SAMPLE LOAD_Validation_02_Sample Holds" uses:
  WHERE r.Sample_date >= DateAdd('m', -9, Date())   (9-month window, date-only)
Current code at line ~421 uses:
  r.[Sample_date] >= DATEADD(month, -6, GETDATE())  (wrong: 6-month window, datetime)

Change ONLY that predicate to match legacy exactly:
  r.[Sample_date] >= DATEADD(month, -9, CONVERT(date, GETDATE()))

That is: -6 → -9, and GETDATE() → CONVERT(date, GETDATE()) for date-only comparison (matches legacy Date()).

Do NOT change:
- the JOIN, the ORDER BY, the Cancelled filter, the customer-number match, or any other predicate
- the ValidateAsync method (the older unused one)
- any other date window elsewhere in the file
Only this one WHERE date comparison in Step 6 (the recent-reviews hold query, ~lines 404-422).

List the exact line changed. Commit: "Fix Bug 198: restore 9-month date-only recent-review hold window (regressed from legacy -9 to -6)".
