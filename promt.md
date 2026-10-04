Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #203, Geoff) — application name text:
1. Change the application name from "CASRR – Credit Assurance Risk Review
   System" / "Credit Assurance Risk Review System" to exactly:
   "Credit Assurance Services RiskReview (CASRR)"
   ("RiskReview" is ONE word.) In exactly these 3 places:
   a) Login screen sentence: "Use your Microsoft credentials to access the
      Credit Assurance Services RiskReview (CASRR)."
   b) Top banner of the app (next to the First Horizon logo, currently
      "CASRR – Credit Assurance Risk Review System") → "Credit Assurance
      Services RiskReview (CASRR)".
   c) Copyright page header subtitle (currently "Credit Assurance Risk Review
      System (CASRR)") → "Credit Assurance Services RiskReview (CASRR)".
2. Copyright page opening paragraph must read:
   "The Credit Assurance Services RiskReview (CASRR) application and all
   associated materials are ..." (rest of the sentence unchanged; note the
   added word "application").
3. Logo (DO NOT CHANGE NOW): the "CAS RiskReview" badge in the Home page
   banner will be replaced with a new PNG from Geoff later. Only identify how
   it is rendered today (image file path / inline SVG / component, file:line,
   size, every place it is used) so the swap can be a single-file change.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- file:line for each of the 3 places + the copyright paragraph, and whether
  they use a shared constant/config for the app name.
- List EVERY other occurrence of the old name in the repo (browser tab
  <title>/metadata, sidebar, footer, PDFs/report headers/footers, email
  templates, backend strings, README/docs, tests). Do NOT change those —
  just list them with file:line; I will confirm with the client.
- Logo discovery as described in item 3.
- If a business decision is needed, STOP and ask with A/B/C options.

PHASE 2: PLAN
- Smallest change: edit the 3 places + paragraph. If an app-name constant
  already exists and is used ONLY by these places, update it there; do not
  create a new global constant that would silently change other places.

PHASE 3: IMPLEMENT
- Exact text, exact casing, single space, "RiskReview" as one word,
  "(CASRR)" at the end. No layout/styling change; if the longer name wraps
  or overflows in the banner on a normal laptop width, report it (don't
  restyle without asking).

PHASE 4: VERIFY + REPORT
- Build frontend (and backend only if touched).

Report:
1. What you found (file:line) for each place, plus the full list of other
   occurrences NOT changed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (login, banner, copyright page)
5. Edge cases: banner text width on narrow/normal screens, mobile/collapsed
   sidebar, logged-out vs logged-in pages
6. Logo: how it's rendered today and exactly which file(s) the new PNG will
   replace
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.
