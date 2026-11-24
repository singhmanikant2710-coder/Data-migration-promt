UAT #263 follow-up (Bug) – Unsaved Scorecard comments disappear from screen after deleting a CRM finding

REPRO (Geoff)
1. Edit the Scorecard comments (Scorecards tab), do NOT save.
2. Go to CRM Findings and delete a saved finding.
3. The Scorecard comments field now shows the previously saved text; the edit is gone from screen. Other comment fields (other tabs) keep their unsaved edits.
4. Clicking Save then brings the Scorecard edit back (it was still staged).

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- What the CRM finding delete does after the server call (CrmFindingsAndRatingsSection.tsx – reload / router.replace / refetch, file:line).
- How each section re-initialises its displayed values when review data reloads (ScorecardsSection.tsx and its hook vs Key Risks, Customer Background, Risk Rating Justification, etc.). Why Scorecards overwrites the screen with the reloaded value while its staged change remains.
- List EVERY section/field with the same pattern (display reset on reload while a staged edit exists) – e.g. Transactions/account delete also reloads.
- Exact root cause with file:line.

PHASE 2/3 – FIX (smallest generic change)
- After a reload, any field with a staged (unsaved) change must keep showing the staged value; fields without staged changes show the reloaded data. Prefer one shared approach over per-field patches; apply it to every affected section found.
- Do not change what is saved, the #233 send-only-edited rules, #177 drafts, #232 version handling, #254 PE behaviour or #263 findings logic.

PHASE 4 – REPORT (in chat, not in a file)
- Root cause, list of affected sections, ADDED / CHANGED (file:line).
- Before/after for: Geoff's repro; edits in several tabs then delete a CRM finding; same with deleting an account (Transactions); no unsaved edits (reload shows fresh data); Save after delete saves the edits once.
- npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
