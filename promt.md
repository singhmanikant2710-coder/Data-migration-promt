Fix Bug #189 in the Covenants UI only.

IMPORTANT:
- Make the smallest possible change.
- Do NOT modify any unrelated tab, component, functionality, API contract, database schema, or shared logic.
- Do NOT refactor existing code.
- First trace the existing save flow before changing Issue 2.

ISSUE 1 — Color:
In CovenantsSection.tsx, "Is Stepped Up Servicing Required" currently has:
Yes = Green
No = Red

Change ONLY the color mapping to:
Yes = Red
No = Green

ISSUE 2 — N/A becomes No:
These 3 fields already have Yes/No/N/A options:
- accuratelyDefinedTracked
- accuratelyCalculated
- breachesMitigated

Currently:
Select N/A → Save → value becomes No.

CovenantsSection.tsx already passes N/A through setField(), so find where the value changes from "N/A" to "No" in the existing save/data flow.

Fix only that conversion so:
Yes → Yes
No → No
N/A → N/A

Do NOT change the existing Yes/No behavior.

After the fix:
- Verify N/A remains N/A after Save and reload.
- Verify Yes/No still work.
- Verify existing Covenants functionality is unchanged.
- Run TypeScript/build/lint if available.
- Review the final diff and ensure only Bug #189 changes are included.

If N/A cannot be persisted because of an actual backend/data-model boolean restriction, DO NOT change the backend/schema. Stop and tell me exactly where that restriction exists.
