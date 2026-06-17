Stop. This is purely a frontend CSS/positioning bug, not a backend issue.

Open this file only:
frontend/src/app/maintenance/naics/page.tsx

Find the SearchableSelect component used for the Division field 
(and Sector field). 

The dropdown/popover currently opens UPWARD and expands to cover 
the full screen height — this is wrong.

Fix: Make it open DOWNWARD only, and constrain its max-height so 
it doesn't cover the full screen (e.g. max-height: 300px with 
internal scroll).

Look for props like `side`, `position`, `align`, or `flip` on the 
SearchableSelect/popover component being used for Division and Sector. 
Set them to force downward opening (side="bottom", disable flip).

Do not touch the backend. Do not touch SelectionsController.cs. 
Do not touch any other file. Only fix the dropdown positioning CSS/props 
in naics/page.tsx (and the shared SearchableSelect component file if 
the fix needs to go there).

Show me exactly which lines you're changing before making the change.
