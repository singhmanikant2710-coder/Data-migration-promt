1: A – 1.1.0 (set in frontend/package.json "version").
2: A – "CASRR v1.1.0".
3: A – no environment next to the version.
4: B – import only the version in a small lib file (e.g. lib/appVersion.ts); do not modify next.config.mjs. Confirm in the production build output that only the version string reaches the client bundle, not the whole package.json (dependency list). If it does, report and propose an alternative instead of shipping it.
Proceed with Phase 3 and 4.
