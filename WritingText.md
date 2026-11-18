#246 test result: the 15/5-minute warnings appeared (please confirm from code that nothing else is expected), the session ended at 8 minutes with sign-out. I had UNSAVED edits in Key Risks when the session ended. After signing in again and opening the same review, the "Restore unsaved changes?" prompt did NOT appear. Expected: the #177 session-expiry draft is kept and offered.

READ-ONLY DISCOVERY first (no edits):
- Trace with file:line what happens at session end: AuthProvider timer → #177 session-expiring event → draft write (which storage, which key) → sign-out → /logout page → sign-in → Review Form draft check.
- Does anything clear the draft on the way (logout page, MSAL logout, sessionStorage/localStorage clear, auth.session removal, key that includes the user/session id so a new session cannot find it, 24h expiry check)?
- Is the draft written before the redirect happens (async write racing the redirect)?
- Did #246 change any of this path (AuthProvider, msal.ts dev TTL, SessionWarningProvider)? Compare with git HEAD.
- State clearly: regression from #246, or pre-existing #177 behaviour. Exact root cause with file:line.

If it is a #246 regression: fix with the smallest change. If it is pre-existing: report only, do not fix yet.
Report in chat: root cause, regression or pre-existing, ADDED/CHANGED (file:line), npx tsc --noEmit, npm run build, git status. Do not commit or push.
