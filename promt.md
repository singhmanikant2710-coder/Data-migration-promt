There is a build error in the frontend. Fix it.

File: frontend/src/app/load-samples/page.tsx
Line 35: import { getEicNames } from "@/services/api/admin";

ERROR: Export getEicNames doesn't exist in target module 
@/services/api/admin

Read these two files:
1. frontend/src/app/load-samples/page.tsx
2. frontend/src/services/api/admin.ts

Find where getEicNames is actually used in load-samples/page.tsx.
Then check if getEicNames exists in any other service file under
frontend/src/services/api/ folder.

Fix the import to point to the correct service file where 
getEicNames is actually defined.

If getEicNames is not used anywhere in load-samples/page.tsx, 
simply remove that import line.

Do not change any other logic. Show me exactly which line changed.
