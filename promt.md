On QA (deployed, slow network) BOTH issues still reproduce: (1) false-dirty from transaction enrichment (console shows collateralDesc/businessTypeDesc/_update staged on load), and (2) double-save on "Save and leave" (two POSTs). Confirm whether these two fixes were actually applied, and if not, apply BOTH now. Also the temporary DIRTY-DEBUG logs are still present and got deployed — remove them.

1. TRANSACTION ENRICHMENT (Option c — dirty/draft exclusion in reviewDraft.ts): confirm it's applied. The console still shows transactions.<acctId>.collateralDesc / businessTypeDesc / _update making isDirty=true on a clean load. If not applied, apply it: wildcard-ignore transactions.*.collateralDesc/.businessTypeDesc/.purposeDesc and _update in hasDraftableChanges/sanitizeDraftChanges, dropping a transaction bucket that contains ONLY these derived keys, keeping buckets that also have a genuine edit. reviewDraft.ts only. Save payload unchanged.

2. DOUBLE-SAVE (savingRef latch): the "Save and leave" saves twice on QA (slow network widens the isSaving render-defer window). Add a synchronous savingRef (useRef) guard at the top of handleSaveAndLeave and handleRestoreDraft: if (savingRef.current) return; set true before await, reset false in finally. Do NOT modify handleSave. Optionally add && !isSaving to TopChromeBar saveEnabled.

3. REMOVE all TEMP DEBUG UAT#177 instrumentation (the console.trace / console.log / beforeunload+link-click logs in FormChangesContext.tsx and useUnsavedChangesGuard.ts). These must NOT ship.

Show all diffs grouped. Rebuild (npm run build + node --test). Do NOT commit. I'll deploy to QA and re-test both.
