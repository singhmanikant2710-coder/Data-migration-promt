UAT #263 (Bug, High) – CRM Findings: editing Component or Finding Code of a SAVED finding creates a duplicate

REPRO (Geoff)
A saved finding Underwriting / UW-105. Manager edits it to Credit Servicing / CS-105 and clicks Save. Result: both UW-105 and CS-105 exist, with the same comments. Does not happen for findings that were never saved. "This was not an issue in earlier UAT work" – likely a regression.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- CRM Findings tab end to end with file:line: row state (how a saved row is identified – by code? by position? by an id?), what is staged when Component/Code change, the save payload builder in review-info/page.tsx, the backend save (ReviewService + SqlReviewRepository findings upsert/delete), and the key of dbo.[02_CORE_07_Findings] (PK Review_id + Finding_code – confirm; is there a surrogate id column?).
- Exact root cause with file:line: is the old code ever sent/deleted when the code changes?
- Regression check: git log/blame on these files – which commit changed this behaviour (e.g. UAT #233 send-only-edited fields, #232 concurrency, #177 drafts, #235/#254 PE-style changes)? Show the before/after lines.
- Same problem for: changing only Component (does the code change with it?), changing code twice before saving, changing a code to one that already exists on the review (PK clash), deleting a saved finding after changing its code, CRM-00 "No CRM Findings" placeholder rows, CRM Ratings or other tabs that write findings.
- Give me a read-only SQL that lists likely existing duplicates (same Review_id, different Finding_code, identical non-empty Finding_comments) so the business can review them.

PHASE 2 – PLAN (smallest generic change)
- When a saved finding's code changes, the save must REPLACE it: the original row (original code) is updated/re-keyed or deleted + new row inserted – atomically (one transaction), comments/level/category moved, nothing else touched.
- The frontend must track the ORIGINAL saved code per row and send it (e.g. originalCode) when the code changes; backend uses it. Untouched rows and #233 send-only-edited rules stay as today.
- Changing to a code that already exists on the review: block with a clear message (no silent overwrite/merge) – or list options for me.
- Keep #232 version check/bump and #177 draft behaviour.
- No schema change unless unavoidable – ask first. No automatic clean-up of existing duplicates (business will review; SQL list only).
- Do NOT decide silently – list decisions with recommendations.

PHASE 3 – IMPLEMENT (after my answers, if any decisions; otherwise proceed)

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- Root cause + regression commit (file:line), ADDED / CHANGED / REMOVED (file:line).
- On-screen before/after: Geoff's repro (UW-105 → CS-105); change code only; change component + code; change twice before save; change to an existing code; change then delete; new unsaved finding (unchanged); save other tabs without touching findings.
- Read-only verification SQL per Review_id after each case.
- NULL/edge cases: empty comments, CRM-00 placeholder, locked review, save rejected by #232/#249 (nothing written).
- Other screens unaffected (CRM Ratings, reports, memos).
- dotnet build + tests, npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

Thanks Geoff – I'm looking into #263 now as a priority. I'll also share a list of reviews that likely have these duplicate findings, so they can be reviewed before any clean-up.
