Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. Veracode static scan casrr/api-20261005.3 (sandbox casrr/api-qa).
FRONTEND ONLY – do not touch any .NET code.

TASK: Veracode GROUP B – CWE-601 URL Redirection to Untrusted Site (Open
Redirect). 2 Medium, Exploitability "Likely", status NEW, no mitigation.
#65 ReviewInfoSection.tsx:136
#66 TransactionsSection.tsx:266
(Line numbers are from the 5 Oct scan – locate by file + surrounding code.)

Already APPROVED look-alikes (Potential False Positive, mitigation
approved) – use as reference:
#43 page.tsx:1113, #44 page.tsx:566, #62 page.tsx:1247,
#47 CrmFindingsAndRatingsSection.tsx:352
(page.tsx = review/[ecif]/review-info/page.tsx unless you find otherwise.)

Note: both flaws are NEW. The UAT #178 change (returnTo param, whitelisted
via lib/reviewReturn.ts) touched the URL allowlists in ReviewInfoSection,
TransactionsSection, CrmFindingsAndRatingsSection and review-info/page.tsx.
You earlier reported "5 near-duplicate buildSafeInternalUrl copies with
drifting allowlists". Check whether #65/#66 were introduced or changed by
that work (git log -L / git blame on the flagged lines).

Approach:
1. Compare each flagged sink with the approved look-alikes.
   - Same pattern (same helper, same allowlist logic, only internal
     relative paths built from constants + encodeURIComponent query
     params) -> FALSE POSITIVE: no code change; write a ready-to-paste
     Veracode "Mitigate by Design" justification in the same style as the
     approved ones.
   - Different / weaker pattern (any path where a value from the URL,
     API, DB or user input can decide the destination origin or scheme)
     -> REAL: fix by allowing only internal relative paths (must start
     with "/" and NOT "//" or "/\", no scheme, no backslashes), otherwise
     fall back to the current safe default route. Reuse an existing
     helper if one already does exactly this; do not create a 6th copy.
2. Do NOT change navigation behaviour: section tab switching, reviewId/
   sampleId/ecif/section/returnTo params, Save/rebuild URL, deletes, Review
   Home return (UAT #178) must all work exactly as today.
3. Do not refactor or merge the duplicate helpers in this task (report
   only).

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
For each flaw: show the code at file:line, the sink (router.push/replace,
window.location, window.open, <a href>, Link href), every input that flows
into the destination (URL params, API fields, props), git blame/introducing
commit, and classify REAL or FALSE POSITIVE with a one-line reason,
compared side by side with the approved look-alike. If a decision is
needed, STOP and ask me with A/B/C options.

PHASE 2: PLAN – smallest change only for REAL flaws.
PHASE 3: IMPLEMENT – touch only the needed lines.
PHASE 4: VERIFY + REPORT – verify with real functions:
- internal destinations still produced exactly as today (section tabs,
  returnTo=queue/progress/history, Save, deletes);
- hostile inputs never navigate off-site: https://evil.com, //evil.com,
  /\evil.com, \\evil.com, javascript:alert(1), data:text/html,..,
  %2F%2Fevil.com, /%5Cevil.com, whitespace/case variants.

Report:
1. Table: Flaw ID | file:line (current) | sink | inputs | introduced by
   (commit) | REAL/FP | change made OR "Mitigate by Design" text ready to
   paste into Veracode
2. Root cause
3. ADDED lines (file:line)
4. REMOVED / CHANGED lines (file:line)
5. On-screen behaviour before vs after (navigation in Review Form, Review
   Home return, Transactions)
6. NULL / undefined / empty param cases
7. Other screens unaffected + duplicate helpers found (not changed)
8. tsc + npm run build results (lint broken repo-wide – report separately)
9. git status (no harness/temp files; test-data/ not staged)
Do not commit or push.

Note: latest scan is casrr/api-20261007.3 (7 Oct); #65 ReviewInfoSection.tsx:136 and #66 TransactionsSection.tsx:266 are unchanged there. Also note that TransactionsSection.tsx was just modified for the CWE-80 fix (DOMPurify import + help-tip line) – locate the redirect sink by surrounding code, not by line number.
