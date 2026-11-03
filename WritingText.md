Before I accept #232, three follow-ups (report in chat with file:line):

1. Scorecard pre-write removal (ReviewController.cs:99-129): prove it was a true duplicate of ReviewService.cs:1002-1035 for every input – scorecard comments sent alone, with other fields, empty string vs null, and from every caller. If there is ANY difference, restore the original logic but run it AFTER the version check (inside the same flow), not before. Report which you did.

2. New Row_version column impact: search the whole repo (C#, SQL in code, scripts/sql, stored procedures if any) for INSERT INTO dbo.[02_CORE_02_Reviews] without an explicit column list, SELECT * INTO / INSERT ... SELECT * involving Reviews, SELECT * from Reviews mapped to DTOs/DataTables, bulk copy, and any code that copies or moves review rows. List each hit with file:line and whether a rowversion column would break it. Fix only real breakages with the smallest change.

3. Conflict Refresh: keep the #177 draft approach, but when the restore prompt is shown after a 409 refresh, its text must say clearly that "Restore and save" will save your changes over the latest data, and suggest "Decide later" to compare first. Do not change the prompt for other (non-conflict) uses.

Same report rules: ADDED/CHANGED (file:line), builds, git status, do not commit or push.
