Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app – reviewers are losing saved
data. Existing behaviour must not break.

BUG (Geoff, reproduced in QA/Prod, review ALLSTATES WORLDCARGO INC):
1. Open a review whose Collateral = Strong, PSOR = Adequate, SSOR = Weak.
2. Go to the Risk Rating Justification section → Edit → change/delete text
   in the Risk Rating Justification field → Save.
3. Result: Collateral, PSOR and SSOR rating selections revert to
   "Select..." (in the Collateral / Repayment sections) and the summary
   boxes above RRJ change (e.g. all show "Weak" / blank).
Geoff suspects the summary boxes (Committed Exposure, Collateral Rating,
PSOR Rating, SSOR Rating, Max Bank/CAS PD, Risk Recognition Key Findings,
Unsatisfactory Risk Recognition) shown inside the RRJ tab.
Expected: saving RRJ changes ONLY the RRJ text; all ratings stay exactly as
stored.

PHASE 1: READ-ONLY DISCOVERY (no edits)
- Trace with file:line: RRJ section component + the summary boxes
  component, how they read Collateral/PSOR/SSOR, whether anything there
  calls changes.setField / stages values / passes defaults (e.g. a select
  rendered with value "" or defaultValue that fires onChange on mount),
  the merged save payload (page.tsx getMerged / handleSave), the API
  contract, and the backend save path (service + SQL) for the three
  rating columns.
- Find EXACTLY how the ratings get overwritten: are they in the payload as
  "" / null / a default? Is a whole section object sent with missing
  fields that the backend writes as NULL? Does the backend UPDATE set
  columns that were not provided?
- Show the exact payload sent when only RRJ text is changed (log it /
  reconstruct from code).
- Check the same pattern for every other section that shows these summary
  boxes or shares the same save path (Collateral, Repayment, Scorecards,
  CRM Ratings, Key Risks, etc.) and any other field that could be wiped
  the same way.
- Check if there is any audit/history table or rowversion data that would
  let us recover the previous rating values for affected reviews; give SQL
  to find reviews likely affected (e.g. ratings NULL while RRJ has text /
  recently updated) — read-only, don't run against Prod.
- If a decision is needed, STOP and ask me with A/B/C options.

PHASE 2: PLAN — smallest generic fix at the root cause:
- Display-only components (summary boxes) must never stage/save values.
- The save payload must contain only fields the user actually changed;
  the backend must not overwrite columns that were not provided (treat
  "absent" differently from "explicitly cleared").
- Fix once in the shared path so every section is protected; no
  per-field special cases.
- No schema change. Any data-recovery/report SQL as idempotent scripts
  under scripts/sql/ (report-only, no automatic updates).

PHASE 3: IMPLEMENT — touch only what is needed.

PHASE 4: VERIFY + REPORT — build backend + frontend; verify with real
functions/payloads: RRJ-only save keeps ratings; changing a rating still
saves; explicitly clearing a rating still clears it.

Report:
1. Root cause (file:line) + the exact bad payload/SQL
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (RRJ save, rating save, other sections)
5. NULL / edge cases: ratings already NULL, user clears a rating on
   purpose, RRJ save with no text change, two sections edited in one
   save, locked/approved review save path, draft restore (localStorage)
6. Other sections/fields checked and confirmed safe
7. Data recovery: affected-review SQL + whether old values are
   recoverable
8. Build results + git status (no harness/temp files)
Do not commit or push.
