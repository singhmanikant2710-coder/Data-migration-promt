Hi Geoffrey, thanks — those screenshots are really helpful, and they narrow things down.
Two observations:
The upload clearly ran against the Test environment and reported "Saved 86,875 rows to Data Mart Trial" — and 86,875 is exactly the row count I see in the table. So the load definitely landed in the right place and the rows are there.
That means the issue isn't a missing or rolled-back load — it's that specific columns came through empty while others (like InterestType, which we can see had values) came through fine.
The most likely cause is at the CSV-to-column mapping step. The upload screen lists the expected headers, and they must match exactly. Notably, the expected header is spelled InternalPortcat (lowercase "c"), whereas the field is often written as InternalPortCat. If the CSV header differs from the expected header even by case or spacing, that column would be skipped and land as NULL while every other column loads normally.
Could you ask John to check two things in the source CSV he uploaded:
Do the header names for InternalPortcat and IntRepCMLSubCategory match the expected headers exactly as listed on the upload screen?
Do those two columns actually contain values in the source file, or are they empty there too?
That should tell us quickly whether it's a header mismatch or simply missing source data. Happy to verify from my side once he confirms.
Thanks, Manikant
