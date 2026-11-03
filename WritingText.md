Q1: A – reject a save/delete with no version with 409 (same message), once the DBA script has run.
Q2: A – accept: any writer that changes the Reviews row bumps the version; open users get 409 and must refresh.
Q3: A – always treat as conflict (two tabs, lost-response retry); the user's edits stay on screen.
Q4: A – Refresh reloads the latest review and re-applies the user's unsaved edits (only the fields they edited, as #233). If a field the user edited was also changed on the server since they opened it, list those field names in the dialog after refresh so the user can check before saving again. Keep this small; if it needs large changes, report instead of building it.
Deployment guard: until the rowversion column exists, the code must behave exactly as today (no check, no error). Report how this is detected (e.g. once at startup or cached), with file:line.
Proceed with Phase 3 and 4.


