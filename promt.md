Bug 196 — READ-ONLY confirm. NO edits. Open only these, answer 3 questions, STOP.

FILE 1: backend/src/Casrr.Api/Controllers/SelectionsController.cs
→ Lines 162-195 (the PUT library/{id} method).
Q1. Inside this method, before _repo.UpdateAsync is called, is there ANY call to AddAsync / InsertAsync / a "create if not exists" branch? Paste those lines if yes.

FILE 2: backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs
Q2. Open the FULL UpdateAsync method (not just the SQL). Does it do a SELECT-then-INSERT, MERGE, or any INSERT anywhere? Paste the whole method body.
Q3. What is the actual PRIMARY KEY of table dbo.[03_LIBRARY_09_Selections]? Search the codebase (migrations, .sql schema files, or EF config) for the CREATE TABLE / PK definition of 03_LIBRARY_09_Selections and paste the PK columns.

Answer Q1, Q2, Q3 with file paths + line numbers. Then STOP.
