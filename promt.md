The CustomerInfo cascade fix did NOT resolve it — popups still appear in 3a (Edit clicked, no typing), 3b (Edit NEVER clicked), and 5 (browser close, clean). Since it happens even WITHOUT clicking Edit, there is a second source that static analysis of the 86 staging sites did not find. Add temporary instrumentation to catch it, then report — do NOT fix yet, just instrument and tell me what's staged.

1. In FormChangesContext, add temporary console logging: whenever `changes` becomes non-empty OR whenever isDirty flips to true, log:
   console.log("[DIRTY-DEBUG] isDirty=true, staged keys:", JSON.stringify(changes), "sanitized:", JSON.stringify(sanitizeDraftChanges(changes)));
   Also log a stack trace at the moment the FIRST staging happens on a clean load:
   console.trace("[DIRTY-DEBUG] first staging");

2. Keep this instrumentation temporary and clearly marked (// TEMP DEBUG UAT#177 — REMOVE).

3. Tell me exactly what to do: open a review, do NOT click Edit, and watch the browser console. Report what I should look for.

Show me the instrumentation diff. I'll run it, open a review WITHOUT clicking Edit, and paste the console output so we can see the exact staged key(s) and where they come from.
