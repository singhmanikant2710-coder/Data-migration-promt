Fix the false-dirty root cause in CustomerInfoSection.tsx. The segment→unit/market cascade effect (lines ~235-247 and ~264-273) fires on data load, not just user edits, because the "skip initial" guard (prevSegmentRef) records "" during the pre-load pass instead of staying null — so when the payload resolves and segment flips "" → real value, the cascade wrongly stages unit:"" and market:"".

FIX (root cause, minimal): Make the cascade fire ONLY on a genuine USER-initiated segment change, not on the initial data-load flip. Options:
- Initialize prevSegmentRef to the resolved segment value once the review payload has loaded (not to the pre-load ""), OR
- Guard the cascade so it does not run until the review data has resolved (e.g. skip while the payload/segment is still in its initial/loading state), OR
- Only run the clear when the segment change originates from the user's onChange handler, not from the effect reacting to a data-load-driven value change.

Pick the cleanest option that:
1. Keeps the genuine cascade behavior (when a user actually changes Segment in Edit mode, Unit/Market DO clear).
2. Does NOT stage unit:""/market:"" on load or on Edit-click without a real segment change.
3. Does not touch the 84 other staging sites or the draft layer.

After the fix: on a clean open (even with Edit clicked), no customerInfo.unit/market should be staged, isDirty stays false, and neither popup appears. When the user actually changes Segment, the cascade still clears Unit/Market and the form correctly becomes dirty.

Show the diff. Do NOT commit. I'll re-test TEST 3 and TEST 5.
