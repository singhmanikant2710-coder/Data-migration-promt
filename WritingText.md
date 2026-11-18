UAT #245 – Home screen: update the "v1.0 CAS RiskReview" label to the current version

REQUIREMENT
The Home screen shows "v1.0 CAS RiskReview". It must show the current version.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- file:line of the label and any other place showing a version or the old name "CAS RiskReview" (footer, login page, About, PDF footers, page titles).
- Where a version number exists today: frontend/package.json "version", backend assembly/csproj Version, git tags, release branch names (release/1.0.0), pipeline variables (azure-pipelines/, pipelines/), environment variables. Report each value.
- Note the app was renamed to "Credit Assurance Services RiskReview (CASRR)" – check how the name appears elsewhere on screen.

PHASE 2 – PLAN (smallest generic change)
- One source of truth for the version, read at build time (e.g. package.json version exposed as NEXT_PUBLIC_APP_VERSION), so the label never needs a code edit again – only the version bump at release time.
- Label format and the version number itself: do NOT decide silently. List options with a recommendation for me:
  1. Version number for this release (e.g. 1.1.0) and where it should be bumped.
  2. Label text: "v1.1 CASRR", "CASRR v1.1.0", or keep "CAS RiskReview" wording.
  3. Show the environment (Dev/QA) next to it or not (there is already a "Local" badge in the header).
- No backend change unless the version must come from the API – ask first.

PHASE 3 – IMPLEMENT (after my answers)
- Touch only the needed code.

PHASE 4 – REPORT (in chat, not in a file)
- ADDED/CHANGED (file:line), before/after label, how to bump the version for the next release.
- npx tsc --noEmit, npm run build (dotnet build only if backend touched).
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
