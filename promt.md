Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (New Functionality, Serial 178, Geoff):
Screen: Review Form → "Review Home" button.
Current: "Review Home" always goes to one fixed screen (confirm which in Phase 1).
Expected: "Review Home" returns the user to the main-menu screen they opened
the Review Form from — Review Queue, Review Progress, or Review History — with
the same filters (and sort/page/page size, if those screens have them) as when
they left. Like a browser Back, but reliable.

Decided approach (do not use router.back() / history.back()):
1. Origin: every place in Review Queue / Review Progress / Review History that
   opens the Review Form adds a query param returnTo with a WHITELISTED key
   only: "queue" | "progress" | "history". Review Form maps the key to the
   route. Any other/missing/invalid value → current Review Home destination
   (byte-identical to today). Never accept a raw URL/path (no open redirect).
2. Filters: each of the 3 list screens saves its current filter state
   (filters + sort + page + page size, whatever exists) to sessionStorage
   under its own key (e.g. casrr.reviewList.progress.v1) whenever it changes,
   and restores it on mount. Wrap all storage access in try/catch; on missing
   or corrupt/old-shape data, fall back silently to today's defaults. Validate
   restored values (e.g. a restored sample/status option that no longer exists
   → drop it, use default). If these screens already keep filters in URL
   search params, reuse that instead and tell me.
3. returnTo must survive inside the Review Form: Edit/Save/Cancel, Email,
   memos, page refresh, and any next/previous-review navigation if it exists.
4. Existing unsaved-changes guard (if any) must still fire before leaving
   via Review Home. Do not change button label or position.
5. Other entry points to the Review Form (Load Samples, Reports, direct
   link, new tab, bookmarks, etc.) keep today's behaviour.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- Trace the full path end to end with file:line: Review Home button handler,
  Review Form route/params, every link/router.push into the Review Form
  (from all screens, not just these 3), and how each of the 3 list screens
  holds filter/sort/paging state (useState, URL params, context, shared hook).
- Identify shared components/hooks between the 3 list screens and anything
  else that uses them.
- List every entry point to the Review Form and what returnTo it will get.
- Report existing bugs/risks noticed; don't fix outside this task.
- If something is ambiguous (e.g. filters already in URL, a 4th main screen
  opens the form, filter state lives server-side), STOP and ask me with
  A/B/C options + trade-offs before editing.

PHASE 2: PLAN
- Smallest correct, generic change. One small shared helper (e.g.
  lib/reviewReturn.ts: whitelist map + build/parse returnTo, and a
  usePersistedFilters hook or save/load helpers) reused by all 3 screens —
  no copy-paste logic.
- Shared code: change only if every caller is safe; otherwise scope it.
- No backend/API/DB change expected. If you think one is needed, stop and ask.

PHASE 3: IMPLEMENT
- Touch only what this task needs. Match each file's existing style.
- Default behaviour of every list screen on a fresh session (no stored
  state) must be identical to today.
- Clean up any temp scripts/harness files you create.

PHASE 4: VERIFY + REPORT
- Build backend and frontend (report file-lock errors separately from real
  compile errors).
- Verify with real functions where possible (whitelist parse, save/restore,
  corrupt storage).

Report:
1. Root cause / what you found (file:line), plus any other issues noticed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen behaviour: before vs after, for each of the 3 screens and for
   other entry points
5. NULL / empty / edge cases and the result for each: returnTo missing,
   invalid, tampered (e.g. a full URL); sessionStorage empty, corrupt, or
   unavailable; restored filter value no longer valid; review status changed
   so it no longer matches the restored filter; page number beyond new
   total; opened in new tab; page refresh on Review Form; after Save/Email
2. Other screens/reports checked and confirmed unaffected
7. Build results
8. git status: which files changed (flag artifacts that are not yours)
Do not commit or push.
