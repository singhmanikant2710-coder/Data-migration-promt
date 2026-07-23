Hi Geoffrey, thanks for sharing that — it's a useful clue.
If John could see values in InterestType during the upload but they're empty now, the data likely did arrive and then got wiped afterwards. A few common causes worth checking with him:
Environment mismatch — the upload may have landed in a different database than the one we're querying. Could you ask John to confirm the exact server and database name he uploaded to? I'm checking [CAS_RiskReview_Test] on the QA server and the Dev equivalent, and both show the same empty columns.
The load was rolled back or overwritten — if the process ran inside a transaction that wasn't committed, or if a subsequent refresh/truncate-and-reload ran afterwards with a source file that didn't include those columns, the values would disappear even though they were visible mid-process.
Column mapping during load — if the staging import maps by column position rather than name, values can land in the wrong columns and leave the intended ones NULL.
To help narrow it down, it would be useful to know from John: which database he loaded, whether the process completed and committed successfully, and whether anything ran against the table after his upload.
Happy to re-run my check against any environment he points us to. On our side the application code is ready and verified — as soon as those columns hold data, both #127 and #128 will work with no further changes.
Thanks, Manikant
