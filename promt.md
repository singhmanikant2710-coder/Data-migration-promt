Two false-positive popups appear even when the user made NO edits (clean Review Form):
1. The app's styled "Unsaved changes" modal (nav guard) — fires on Home/Review Queue click with no edits.
2. The browser native "Leave site? Changes you made may not be saved" (beforeunload) — fires on tab/browser close with no edits.
Both are driven by isDirty being TRUE on a clean, freshly-loaded form. Diagnose the false-dirty. READ-ONLY, no edits. Answer, STOP.

1. When the Review Form loads with NO user input, what gets staged into the changes context so hasDraftableChanges(changes) returns true? Trace exactly which staged key(s) appear on a clean open.
2. Check EVERY section's mount/init effect, the Edit toggle, and any field that self-initializes (dropdown defaults, dates, IDs, tab state, "Select..." placeholders becoming values). List every place that stages a value WITHOUT a real user edit.
3. Known traps already handled: transactions empty-bucket, repayment.analysis.activeDiscussionTab. Find any OTHERS not in REVIEW_DRAFT_IGNORED_PATHS / sanitizeDraftChanges.
4. Specifically the Review Info / Customer Info sections (where it happened): does anything stage on load or on Edit click there?
5. Also: the beforeunload guard in useUnsavedChangesGuard.ts — does it register beforeunload unconditionally, or only when dirty? Confirm it should register ONLY when isDirtyNow() is true, and unregister when clean. If it's always registered, that itself causes the native popup even when clean.
6. Report the exact staged key(s) making it falsely dirty, AND confirm whether the beforeunload handler is gated on dirty state.

Report the false-dirty keys + whether beforeunload is dirty-gated. Do NOT fix yet.
