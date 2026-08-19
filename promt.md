READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

Two new issues to diagnose:

=== A — Horizontal scroll missing on the Select Sample grid ===
1. Show the wrapper/container around the Select Sample DataTable (the outer div(s) that hold the table). Show their className — specifically overflow-x, width, or any table-layout related classes.
2. Is there a fixed/min table width that would need horizontal scroll when columns change, or does the table just shrink/grow with column count?

=== B — "YYYY" not showing after "MM/DD" in date input ===
3. Show the DateInputWithCalendar component (or wherever the Start/End date input renders) — its className, width, and placeholder/format. Show why only "MM/DD" is visible and "YYYY" is cut off (likely a width constraint like w-[6rem] that was already too narrow, or the input format itself).
4. Is this date input width shared/used elsewhere, or specific to this grid's Start/End columns?

Read once. Findings only. No edits.
