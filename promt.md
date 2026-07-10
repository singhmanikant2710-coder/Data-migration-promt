Bug: The findings/library calls are all returning 401 Unauthorized. In the Network tab, every "library?component=..." request fails with 401, so descriptions aren't loading and the dropdown shows only codes.

The issue: the get() helper used in useCrmFindings.ts to fetch findings/library is NOT attaching the auth token, unlike the app's standard API helper (api.ts) which other working calls use (review, cas-users return 200).

READ-ONLY diagnostic first:
1. Show me exactly how useCrmFindings.ts calls the findings/library endpoint — which fetch/get helper does it use, and does that helper attach the auth token/headers?
2. Compare with how the existing working lookups (e.g., the ones that return 200 like listLibrary or the reviews API in services/api) attach auth.
3. There is already a listLibrary import in this file (I saw it in the imports). Does listLibrary hit findings/library WITH auth? If so, we should use listLibrary instead of a raw get().

Then propose the fix: use the SAME authenticated API helper that the working calls use (or the existing listLibrary function) to fetch findings/library, so the token is attached and we get 200 instead of 401.

Also: the library endpoint is being called many times (once per component, repeatedly). Ensure the per-component cache prevents duplicate calls — fetch each component's library at most once.

Report findings and proposed fix. Do not edit yet.
