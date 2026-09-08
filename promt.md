The double-save reproduces on the deployed (Azure, slow) environment but not locally — this is a race condition: isSaving (useState) updates render-deferred, so on a slow network the window for a duplicated click/event is wider, letting two handleSave calls through before the button disables. Local's fast response hides it. This is a real production risk (duplicate POSTs → duplicate rows for Insert trackers).

Apply the minimal re-entrancy guard (Hypothesis A fix). Do NOT modify handleSave. Show diff, do NOT commit.

FILE: page.tsx (ReviewInfoContent)
1. Add a synchronous savingRef (useRef<boolean>): 
   - At the very top of handleSaveAndLeave: if (savingRef.current) return; then savingRef.current = true;
   - Also guard handleRestoreDraft the same way (it also calls the save path).
   - Reset savingRef.current = false in a finally block after the await completes (success OR error), so a failed save can be retried.
   A ref flips SYNCHRONOUSLY, so it blocks the same-batch/second click that the render-deferred disabled={isSaving} misses.

2. Optionally (pre-existing, separate): add `&& !isSaving` to the TopChromeBar saveEnabled expression so the toolbar Save can't be re-entered mid-save. One expression change, does not touch handleSave.

Do NOT change handleSave itself, the save payload, or any section. Only add the ref latch to the two handlers (+ optional toolbar guard).

Show diff. Rebuild. Do NOT commit. I'll deploy/test to confirm the double-save is gone.
