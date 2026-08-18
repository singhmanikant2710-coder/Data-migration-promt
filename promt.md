READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

Files (read each ONCE, findings only, no edits):
- frontend/src/components/pdf/InitialMemoPDF.tsx
- frontend/src/components/pdf/FinalMemoPDF.tsx

These two reports share the same 3 issues. Show me the following for BOTH files.

=== ITEM 1 — Font size 11 for blue highlighted values ===
1. Find the "blue highlighted values" — likely values in a table styled with a blue color/background. Show the style controlling these value cells' fontSize, and the current value. Identify exactly which style object(s) render these values so I can change only them.
2. Flag if that style is SHARED with other (non-blue) content.

=== ITEM 2 — Scorecard ID wrap in the Scorecard Assessment table ===
3. Find the Scorecard Assessment table. Show the Scorecard ID value cell JSX + how the ID string is produced. CRITICAL: look for any code that inserts SPACE characters after hyphens (e.g. .replace(/-/g, "- ") or similar), or \u200B, or \n — the ticket says spaces were added after hyphens to force wrapping. Show the exact string transformation.
4. Show the Scorecard ID column width style in this table (fixed %/flex) and the cell's wordBreak / wrap props.

=== ITEM 4 — Scorecard ID wrap in the Account Information table ===
5. Find the Account Information table. Show its Scorecard ID value cell JSX + the same string transformation (spaces after hyphens? \u200B? \n?).
6. Show that column's width style and wrap props.

=== SHARED CHECK ===
7. Is there a SHARED formatScorecardId (or similar) helper used by both tables / both files that does the hyphen-space insertion? Show it. This matters because fixing it once may fix multiple spots — or breaking it may affect several places.

CONSTRAINTS:
- Read each file ONCE. Findings only. No edits.
- For each Scorecard ID spot, I specifically need to see whether the wrapping is forced by inserted spaces/characters (which we'd remove and replace with proper wrapping) vs a column-width issue.
- Flag every SHARED style/helper.
