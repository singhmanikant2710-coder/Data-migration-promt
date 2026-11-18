Yes – go with option 1 (read the version on the server in app/page.tsx and app/search/page.tsx, pass it to CASRRHome as a prop).
- If the prop is missing (any other use of CASRRHome), show "CASRR" without a version – never the old "v1.0".
- Keep package.json / package-lock.json at 1.1.0 as already changed.
- Re-check the production client bundle the same way: only the string "1.1.0" may appear, no other package.json content.
Report ADDED/CHANGED (file:line), before/after label, npx tsc --noEmit, npm run build, git status. Do not commit or push.
