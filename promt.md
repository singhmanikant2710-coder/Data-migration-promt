Bug 196 — READ-ONLY diagnosis. NO edits, NO planning loops. Open the specific files below, answer 4 questions, STOP.

Do NOT grep the whole repo. Do NOT explore. Just open these files directly:

FILE 1 — Backend controller:
C:\Users\CC438\source\repos\fhn-casrr\backend\src\Casrr.Api\Controllers\SelectionsController.cs
→ Find the UPDATE/edit endpoint for a selection (the one that saves Reporting name / CRM Component).
→ Report: HTTP verb + route + which repository method it calls.

FILE 2 — Repository:
C:\Users\CC438\source\repos\fhn-casrr\backend\src\Casrr.Infrastructure\Repositories\SelectionRepository.cs
→ Find the method the controller calls on save/edit.
→ Report the exact SQL (or EF calls). Is it INSERT, UPDATE, or MERGE/UPSERT?
→ Report the WHERE clause / key columns used to match the existing row.

FILE 3 — Interface (for signature only):
C:\Users\CC438\source\repos\fhn-casrr\backend\src\Casrr.Application\ISelectionRepository.cs
→ Report the method signature used for the edit.

FILE 4 — Frontend Selections maintenance component (under app\maintenance — open the Selections one):
→ Find the save handler for editing Reporting name.
→ Report: apiClient call — POST or PUT, the URL, and the exact payload keys sent.

ANSWER ONLY THESE 4, with file paths + line numbers:
Q1. Frontend: POST or PUT? URL? payload keys?
Q2. Controller: verb + route + repo method called?
Q3. Repository: INSERT vs UPDATE vs UPSERT? What key columns are in the WHERE / match clause?
Q4. Does the match/key include "Reporting name" (the field being edited) itself?

Then STOP. Do NOT propose fixes. Do NOT open other files.
