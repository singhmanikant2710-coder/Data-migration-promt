TEST 3 FAILED — the "Unsaved changes" popup appears even when the user made NO edits (clean state). This is a false-positive dirty detection. Diagnose which staged change is being added without a real user edit. READ-ONLY, no edits. Answer, STOP.

1. The dirty check uses hasDraftableChanges(changes). When the Review Form loads (or when Edit is clicked) with NO user input, what gets staged into the changes context that makes it non-empty? 
2. Check every section's mount/init effect and the Edit-mode toggle: does any section stage a value on mount, on Edit click, or on first render (e.g. initializing a field, a dropdown default, a date, a tab state)? List every place that calls the staging/setChanges without a real user interaction.
3. The known false-dirty traps were: transactions empty-bucket after delete, and repayment.analysis.activeDiscussionTab. Are there OTHERS not covered by sanitizeDraftChanges / REVIEW_DRAFT_IGNORED_PATHS? 
4. Specifically for the Customer Info section (where the popup appeared): does anything stage a change on load/edit there?
5. Report exactly what staged key(s) make the form falsely dirty on a clean open, and whether the fix is (a) add those paths to REVIEW_DRAFT_IGNORED_PATHS / sanitize, or (b) prevent them from staging in the first place.

Report the exact staged keys causing false-dirty. Do NOT fix yet.
