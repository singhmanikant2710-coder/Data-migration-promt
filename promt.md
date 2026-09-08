NEW ISSUE (separate from the false-dirty, which is now fixed): clicking "Save and leave" saves the review TWICE — two "Review saved" success toasts appear for a single click. This is a duplicate-save, violating the "no duplicate records" acceptance criterion. READ-ONLY, no edits. Answer, STOP.

1. Trace the "Save and leave" button handler exactly. What does it call, in what order? (e.g. handleSave() then router.push()? Or something else?)
2. Is handleSave being invoked more than once for a single "Save and leave" click? Check:
   a. Does the button handler call handleSave directly AND something else (a flush, the guard, an unmount effect) also call handleSave / a save?
   b. On navigation/unmount after Save-and-leave, does the FormChangesContext unmount-flush or the useUnsavedChangesGuard fire a second save?
   c. Is there any beforeunload / pagehide handler that also triggers a save?
3. Is the "Save and leave" button missing a double-invocation guard (e.g. isSaving check, or disabling during save), so a single logical action calls save twice?
4. After the first save succeeds, does clear() run and isDirty become false BEFORE the second trigger — or does the second trigger fire on still-stale dirty state?
5. Identify exactly where the second save originates and the minimal fix (e.g. Save-and-leave should call the existing save ONCE, wait for success, then navigate; and the guard/flush must NOT re-save when a save is already in progress or just completed).

Report the two call sites that both trigger a save on one "Save and leave", and the minimal fix. Do NOT fix yet.
