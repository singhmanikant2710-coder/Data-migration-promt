READ-ONLY. Find how to make the Top Strip's 60+ DPD % and Cash Collections % follow the Principal/Gross dropdown (principalGrossByLabel), the same way the middle-panel tiles already do via computeConsumerFinancePercentOverride. Quote with paths.

REQUIREMENT (confirmed vs legacy): In legacy, when a customer has BOTH Principal and Gross values, changing the Principal/Gross dropdown CHANGES the displayed % (live). The middle-panel tiles already do this via computeConsumerFinancePercentOverride(label, values, principalGrossByLabel, principalGrossOptions). But the Top Strip does NOT — it does pickExact(server value) then falls back to the raw calc, ignoring the dropdown.

1) Quote where the Top Strip tiles are rendered and where effectiveVal is computed (edit/page.tsx ~L4160-4175 — the middle-panel path that calls computeConsumerFinancePercentOverride). Is there a separate Top Strip render path that does NOT call the override?
2) Quote the Top Strip tile render (the one showing 60+ DPD % / Cash Collections % in the green Summary strip). Does it use monthSummaryColumns' render (which does pickExact), and is that where we'd add the override?
3) computeConsumerFinancePercentOverride uses principalGrossByLabel (dropdown state) + correct denominators (Cash Coll→prior, 60+DPD→current, NetC/O→AvgTTM). Confirm it's already imported/available in the Top Strip render scope.
4) EXACT FIX: In the Top Strip render for the ConsumerFinance percent tiles (Cash Collections %, 60+ DPD %), apply computeConsumerFinancePercentOverride (same as middle panel) BEFORE the pickExact/server value, so the dropdown drives the Top Strip too. Where exactly?
5) Does this need the guard (Option b) at all, or does applying the override make the guard unnecessary? (If the override runs on the Top Strip AND the recompute-loop clobber is the issue, we may still need to prevent the loop from writing wrong-basis per* — OR the override on render supersedes it since render happens after.)

OUTPUT:
- A) Top Strip render path for the % tiles, quoted. Does it call computeConsumerFinancePercentOverride today?
- B) Where to add the override in the Top Strip render (exact location), quoted.
- C) Is computeConsumerFinancePercentOverride available in that scope?
- D) Interaction with the recompute-loop clobber: does render-time override win regardless, or do we still need the guard?
- E) Recommended: apply override to Top Strip render (dropdown-follow, legacy parity). Confirm it's generic (works when both Principal & Gross values exist) and safe for View/Report.
- No fix. Findings only.
