Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In SaveCrmFindingsAsync there is a bug: the SQL is defined as a const string named 
"upSql" (the IF EXISTS UPDATE ELSE INSERT upsert), but inside the loop the 
SqlCommand is created using a variable named "insSql":

    using var cmdIns = new SqlCommand(insSql, conn)

This uses the wrong SQL. 

FIX:
1. Change "new SqlCommand(insSql, conn)" to "new SqlCommand(upSql, conn)" so it 
   uses the correct upsert SQL.
2. If a separate "insSql" variable is still defined anywhere in this method (a 
   leftover plain INSERT from the previous version), REMOVE it so only "upSql" remains.

After the change, the loop must execute the upSql (IF EXISTS UPDATE ELSE INSERT) 
for each finding.

Also: temporarily make the swallowed exception visible — in the per-row catch, 
keep the _logger.LogError as is (good). 

Modify ONLY SqlReviewRepository.cs. If any other file needs changing, STOP and tell me first.
