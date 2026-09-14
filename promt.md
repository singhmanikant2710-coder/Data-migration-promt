Apply the fix. Show diffs, rebuild, run node --test, do NOT commit.

FILE: FormChangesContext.tsx
1. In clear(): set changesRef.current = {} synchronously alongside setChanges({}), so isDirtyNow() is immediately accurate right after a save completes (no stale mirror via the useEffect).

FILE: page.tsx
2. In handleSaveAndLeave: close the modal (setPendingNavHref(null)) BEFORE evaluating whether to navigate, so even if the dirty check were ever wrong again, a retry click is impossible.
Keep the savingRef latch and !isSaving on saveEnabled (still correct for genuine double-clicks).

Do NOT touch handleSave or the payload. ~3 lines total across 2 files.
