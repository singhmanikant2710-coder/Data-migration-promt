READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

The user wants: keep the create-mode fields at a readable size (NOT shrink them), and let the grid SCROLL horizontally so EIC/Type/Target and the Save/Cancel buttons are fully reachable (instead of shrinking everything to fit).

Show me ONLY (no edits):
1. Current isCreating widths for EIC Name, Type, Target (BU) (should be w-40 now). Show them.
2. Current min-w on the Select Sample grid inner div (should be min-w-[900px] now).
3. The Save/Cancel action cell at the end of the create/edit row — its JSX and width. Is it INSIDE the same scrollable grid, or outside? This matters: if it's inside the min-w container, scrolling will reach it; if outside, it won't.
4. Add up the create-mode row total with readable widths: Sample Name (est) + Start(160) + End(160) + EIC + Type + Target + Save/Cancel. I want to set min-w slightly ABOVE this total so the scroll reaches the last button.

Read once. Findings only. No edits.
