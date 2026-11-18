UAT #246 – Warn users 15 and 5 minutes before the automatic 4-hour session closure

REQUIREMENT (Geoff)
Users work in CASRR all day. The system closes the session automatically after 4 hours. Show a warning 15 minutes and 5 minutes before the closure so users can save their work. Existing Review Form save features (#177 autosave/draft, unsaved-changes guard, session-expiry draft) must keep working.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Find what actually causes the "4-hour automatic closure", with file:line and config values:
  - MSAL config (frontend msal setup, token lifetime handling, acquireTokenSilent, interaction-required handling);
  - any app-level idle/absolute session timer in the frontend or backend (cookie lifetime, JWT validation, ClockSkew);
  - API/infra settings in the repo (appsettings, pipelines, App Service/Front Door/APIM notes);
  - if it is not in the repo (e.g. Entra ID Conditional Access sign-in frequency), say so clearly.
- Is it an ABSOLUTE limit (4 hours after sign-in) or IDLE (4 hours without activity)? Can the app know the exact closure time (token claims auth_time/exp, sign-in timestamp)?
- What happens today at closure: redirect to login, API 401, blank page? How does #177 session-expiry draft persistence handle it (file:line)?
- Existing toast/dialog components to reuse.

PHASE 2 – PLAN (smallest generic change)
- One app-wide session-warning component (mounted once in the root layout), not per page.
- Warning at 15 minutes and again at 5 minutes before closure: a dialog that says when the session will end, with "Save now" (only on the Review Form when there are unsaved changes, using the existing save) and "OK". Must not block or discard edits.
- Closure time computed from a reliable source (token/sign-in time), not from page load. Must work with several tabs open (warn once per tab is OK, but times must agree), after laptop sleep, and after the token is silently refreshed.
- Do NOT decide silently – list options with a recommendation:
  1. If the session CAN be extended (e.g. silent token renewal), offer "Stay signed in"? If it cannot (policy), say so in the dialog instead.
  2. Exact dialog wording for 15 and 5 minutes.
  3. Show warnings on every screen or only when there are unsaved changes?
- No backend, schema or SQL change unless discovery proves it is needed – ask first.

PHASE 3 – IMPLEMENT (after my answers)
- Touch only the needed code. #177, #232 conflict dialog, #239/#240 prompts and the navigation guard stay unchanged.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- Root cause of the 4-hour closure (file:line or "outside the repo"), ADDED/CHANGED (file:line).
- How to test without waiting 4 hours (a dev-only override of the session length or warning offsets, off in QA/Prod).
- On-screen before/after: warning at 15 min, at 5 min, Save now with unsaved changes, OK, closure itself; two tabs; laptop sleep; token silently refreshed.
- npx tsc --noEmit, npm run build, dotnet build if touched.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
