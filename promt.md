There is a TypeScript build error blocking the pipeline.

File: frontend/src/models/templates.ts
Line 6: import type { SensitivityDto } from "@app/commercial/types";

ERROR: Cannot find module '@app/commercial/types'

Read these files:
1. frontend/src/models/templates.ts
2. Search if SensitivityDto is used anywhere in templates.ts

Fix:
- If SensitivityDto is NOT used anywhere in templates.ts, 
  remove that import line only.
- If SensitivityDto IS used, check if it exists in any other 
  file under frontend/src/ and fix the import path.

Do NOT modify any other file.
Do NOT change any logic.
Show me exactly which line changed.
