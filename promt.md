READ-ONLY. Do NOT edit. Report only.

There is one central save method that handles saving for all review sections. 
Report ONLY:

1. Name and file of that central save method, and show its FULL source.

2. Inside it, show how ONE existing section (e.g. Checklist or Covenants) is 
   handled end to end: how it reads its piece from the payload, and how it 
   writes/upserts to its DB table (the exact INSERT/UPDATE/DELETE SQL, 
   transaction handling, parameters).

3. For dbo.[02_CORE_07_Findings]: list ALL columns, types, nullability, 
   primary key, and any IDENTITY/auto-increment column.

4. Confirm whether the payload section "crmFindingsAndRatings" already arrives 
   into this method but is currently ignored (no write), or not passed at all.

Report only with exact code/schema. No edits.
