READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

Show me ONLY (no edits):
1. Current isCreating widths for EIC Name, Type, Target (BU) right now (after the last edit — should be w-40).
2. Current min-w on the Select Sample grid inner div (should be min-w-[900px] now).
3. The Save/Cancel action cell/column at the END of the create/edit row — show its exact JSX and width class. CONFIRM: is it INSIDE the same <div className="min-w-..."> scrollable container, or is it rendered OUTSIDE that container? (Critical: if outside, horizontal scroll won't reach it.)
4. Is there anything (a fixed action column, an absolutely positioned Save button) that sits outside the scrollable grid?

Read once. Findings only. No edits. I need to confirm the Save/Cancel is inside the scroll area before setting the right min-width.
