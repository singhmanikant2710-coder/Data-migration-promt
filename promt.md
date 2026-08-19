READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

In NORMAL mode the page fits the screen fine. Only in CREATE mode (new sample row) does the grid become too wide and cut off the page — because in create mode the EIC/Type/Target dropdowns are widened AND the inner div has min-w-[1000px].

Show me ONLY (no edits):
1. The current width classes on the EIC Name, Type, Target (BU) cells in CREATE/edit mode (the isCreating widths — earlier set to w-48/w-40). Show them.
2. The min-w-[1000px] div for the Select Sample grid — confirm its current value.
3. Roughly, in create mode, add up: Sample Name + Start date (w-10rem) + End date (w-10rem) + EIC (w-48) + Type (w-40) + Target (w-48) + Save/Cancel buttons. Is the total likely more than 1000px? This tells me whether to reduce the isCreating dropdown widths OR lower the min-w so it fits the screen.

Read once. Findings only. No edits.
