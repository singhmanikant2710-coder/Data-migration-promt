UAT #234 + #236 – Type-to-filter search for Covenant Type and Policy Exception Description dropdowns

REQUIREMENTS
#234 – Covenants tab, Performance Covenants and Monitoring Covenants sections: the Covenant Type dropdown gets a search box at the top; typing part of a value filters the list.
#236 – Policy Exceptions tab: the Policy Exception Description dropdown (52 options) gets the same search behaviour.
Business note: "operate similar to other drop-down fields where the reviewer types partial selections" – the app may already have a searchable dropdown somewhere.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Find any existing searchable/filterable dropdown or combobox already used in the frontend (file:line, which screens use it, which library if any). List its behaviour: matching (contains vs starts-with, case), keyboard support, disabled options, clear, how the selected value is shown.
- #234: file:line of the Covenant Type dropdown in both sections; option source (API/table/static), option shape (code vs description), what value is saved, how it renders inside the covenant rows/grid, read-only/locked state.
- #236: file:line of the Description dropdown in PolicyExceptionsSection.tsx; option source; how duplicate types are greyed out/disabled (#235); what value is saved; read-only/locked state and the 0-row lock.
- How a saved value that is no longer in the option list (legacy value) is displayed today.
- Every other place using the same select/dropdown component – must not change.

PHASE 2 – PLAN (smallest generic change)
- Prefer reusing the existing searchable dropdown if one exists. Otherwise build ONE small reusable component and use it only in these two places. No new npm package unless discovery shows one is already in the project – ask first.
- Behaviour: search box at top of the open list, focus goes to it on open; case-insensitive "contains" match on the displayed text; "No matches" message; arrow keys + Enter to select, Esc to close; clearing the search shows the full list; same look and size as the current dropdown.
- Must keep: the saved value and payload exactly as today; option source and order; disabled/greyed duplicate types in #236 (still disabled when filtered); locked/read-only states; the #235 rules (0-row lock, safe delete, review-level notes); #233 (send only edited fields); legacy values still display.
- Do not change other dropdowns/screens.
- No backend, schema or SQL change expected – if needed, ask first.
- List any decision for me instead of choosing silently.

PHASE 3 – IMPLEMENT
- Touch only the needed code.

PHASE 4 – VERIFY + REPORT (in this chat, do not write the report to a file)
- What already existed (file:line); ADDED (file:line); REMOVED/CHANGED (file:line).
- On-screen before/after for: Covenant Type in Performance and in Monitoring; PE Description; typing partial text (middle of a word, different case); no matches; keyboard selection; selecting a value and saving; disabled duplicate type in PE.
- NULL/edge cases: no value selected (NULL); saved legacy value not in the list; empty option list; special characters in search (&, /, parentheses); locked review (dropdown disabled); new unsaved row vs saved row.
- Confirm saved values in the DB are unchanged in format, and other screens/dropdowns are unaffected.
- dotnet build (if backend touched), npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave untracked/pre-modified SQL scripts and devin-output/ alone).
- Do not commit or push.
