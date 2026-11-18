Approved: apply the #177 draft fix as a separate change (not part of #246).
- In FormChangesContext.tsx, flushDraft must only delete a draft that THIS form instance wrote itself (no edits in this instance = never delete an existing draft from storage).
- This must fix both: the React Strict Mode dev double-mount case, and the production case where the tab is hidden while the review is still loading.
- Keep everything else exactly as today: drafts are still written on edit (2.5 s debounce), on session expiry, on tab hide and on unmount; "Discard draft", "Restore and save", a successful Save and the top-banner Cancel → Discard still clear the draft as they do now (check each with file:line).
- No other files unless strictly needed.
Report in chat: root cause recap, ADDED/CHANGED (file:line), before/after for: open review with an existing draft (dev and prod build), tab hidden while loading, Save clears draft, Discard draft clears it, Cancel → Discard clears it. npx tsc --noEmit, npm run build, npm run test:unit, git status. Do not commit or push.
