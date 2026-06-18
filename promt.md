There is a build error in load-samples/page.tsx.

The function getEicNames is imported from "@/services/api/admin" 
(line 35) and used at line 127, but this function does not exist 
in admin.ts.

Read these files:
1. frontend/src/app/load-samples/page.tsx
2. frontend/src/services/api/admin.ts
3. frontend/src/services/api/cas-users.ts (or any users service file)

Find where EIC/active user names can be fetched from existing APIs.
The comment on line 119 says: 
"EIC lookup options (active users excluding DBA/DAA/CRA)"

Check if there is an existing endpoint or service function that 
returns CAS users or active users list that can be used instead.

Fix options (choose the correct one):
Option A: If an existing function returns user/EIC names, 
  replace getEicNames() with that existing function and fix 
  the import.
Option B: If no such function exists anywhere, add getEicNames 
  function to frontend/src/services/api/admin.ts that calls 
  the correct backend endpoint for EIC user names.

Do not change any other logic. Show me exactly what you change 
and in which files.
