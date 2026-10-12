Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. Veracode static scan casrr/api-20261007.3 (sandbox casrr/api-qa).
FRONTEND ONLY – do not touch any .NET code.

TASK: Veracode GROUP C – CWE-201 Insertion of Sensitive Information Into
Sent Data. 1 Low, Exploitability "Unlikely", status Open, no mitigation.
#64 route.ts:379 (locate by file + surrounding code; find which route.ts –
likely a Next.js API route handler).

Approved look-alike in the SAME file (Potential False Positive, approved):
#21 route.ts:333. Other approved CWE-201 references: #1 api.ts:701,
#2 api.ts:424, #3 api.ts:280, #45 api.ts:583, #4/#5 loginAudit.ts:110/74,
#46 Sidebar.tsx:29.

Approach:
1. Compare #64 with #21 side by side (sink, what data is sent, to whom).
   - Same pattern -> FALSE POSITIVE: no code change; write ready-to-paste
     Veracode "Mitigate by Design" text in TSRV format (Technique,
     Specifics, Remaining Risk, Verification) in the style of #21, with
     exact file:line.
   - Different -> REAL: remove sensitive data (tokens, secrets, cookies,
     auth headers, user PII, raw exception/stack/connection details) from
     what is sent in the request/response/log. Return a generic message to
     the client and keep details server-side only. Smallest change.
2. Must not change: authentication/session behaviour, API responses that
   the frontend relies on (shape and fields), login audit, error handling
   the UI shows today (except removing sensitive details).

PHASE 1 (read-only): show code at #64 and #21, the data flowing into the
sink, where it is sent (browser, backend API, logs, external service),
whether anything sensitive is included, and classify REAL/FP with a
one-line reason. If a decision is needed, STOP and ask with A/B/C options.
PHASE 2: plan the smallest change (only if REAL).
PHASE 3: implement.
PHASE 4: verify with real functions/requests; tsc + npm run build.

Report:
1. Table: Flaw ID | file:line | sink | data sent | destination | REAL/FP |
   change OR Mitigate by Design text (TSRV)
2. Root cause
3. ADDED lines (file:line)
4. REMOVED / CHANGED lines (file:line)
5. Behaviour before vs after (login, API calls, error messages)
6. NULL / empty / error cases
7. Other screens unaffected
8. Build results (lint broken repo-wide – separate)
9. git status (no harness/temp files)
Do not commit or push.
