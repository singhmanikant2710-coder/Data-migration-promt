Before applying Fix 1, confirm these safety points and quote the exact final code you will write:

1) Show the COMPLETE default-month logic block you're about to apply (from "let def = selectedMonthKey;" through "setSelectedMonthKey(def);") so I can verify.

2) SAFETY CHECK — confirm for a DECEMBER fiscal-year customer (e.g. 1ST FRANKLIN / ACA), where arr = the calendar year's months:
   - Does arr[arr.length-1] equal what maxMonthKey used to give? (For December-year, calendar-max = fiscal-latest = same month, so behaviour is unchanged.) Confirm the default month for December-year customers does NOT change.

3) SAFETY CHECK — the "keep user selection if in arr" branch: confirm that on INITIAL load (when selectedMonthKey is empty/from URL), it still defaults correctly (doesn't leave it blank). Walk through: initial load, selectedMonthKey empty → which branch runs → what def becomes.

4) Confirm this effect's dependency array is unchanged and we're NOT introducing a new render loop (setSelectedMonthKey inside an effect that depends on selectedMonthKey could loop). Quote the dependency array. If selectedMonthKey is NOT in the deps, confirm the keep-selection logic still works; if it IS in deps, confirm no infinite loop.

5) Confirm NO other code path or customer is touched — only this default-month computation changes.

Show the final code block + answers to 1-5. Do NOT apply until I confirm after seeing this.
