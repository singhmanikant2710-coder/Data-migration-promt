Yes, apply the same fix to the client-side CSV branch
(admin/monthly-upload/page.tsx:314-321): run expandScientificIfNeeded
only inside the isNumericColumn branch, exactly as in save/route.ts. Text
columns keep the raw trimmed value. Numeric columns behave exactly as
today. Check for any other place in the codebase that calls
expandScientificIfNeeded unconditionally, and list it (fix it only if
it's on an upload path).

Report ADDED/REMOVED (file:line), behaviour on the small-CSV path for:
"01E0", "01E1", "1-30", empty cell (→ NULL), a numeric amount, and an
amount in E-notation ("4.01343E+12" → "4013430000000"). Build result.
Do not commit.
