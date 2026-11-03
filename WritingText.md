Bug found while testing #232 (rowversion column NOT yet added in Dev):
Policy Exceptions: add a row → Save → delete that row (→ Save). The deleted row stays visible on screen. It only disappears after a manual page reload (Ctrl+R). Expected: the row disappears immediately after delete/save, without a reload.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Trace the PE delete flow with file:line: delete click → frontend service → API → response handling → how the row is removed from state / how the section refreshes after delete and after Save.
- Compare with git HEAD (before the #232 changes): did the delete response change (204 → 200 with { concurrencyToken }), and does the frontend now handle the response differently (parsing, success check, thrown error, state update skipped, reload of review data no longer triggered)?
- Check the same for the other three delete paths changed in #232: covenant delete, CRM finding delete, account delete.
- State clearly: regression from #232, or pre-existing behaviour? Exact root cause with file:line.

PHASE 2/3 – FIX (smallest change)
- If it is a #232 regression: fix it so the row disappears immediately after a successful delete, exactly as before #232, while still storing the new concurrency token. Keep the 409 behaviour (row stays + conflict dialog).
- If it is pre-existing: report only, do not fix yet.
- Do not change anything else.

PHASE 4 – REPORT (in chat, not in a file)
- Root cause (file:line), regression or pre-existing, ADDED/CHANGED (file:line).
- Before/after for PE, covenant, finding and account delete: with the column missing (today) and with the column present.
- NULL/edge cases: token null (column missing), delete of an unsaved row (no server call), delete then Save, delete failure (non-409).
- npx tsc --noEmit, npm run build, dotnet build (if backend touched), git status. Do not commit or push.
