Before applying, ONE more critical check on the "Create mode bootstrap" effect (Section B-2), because it calls setSelectedMonthKey(mkParam) and depends on monthKeys/enrichedSeries/rolling24 — which change when the user selects a month. This could reset the selection back to mkParam (the URL month, e.g. 202512) AFTER the user picks 202605, undoing Fix 1.

Quote the FULL "Create mode bootstrap" effect (entire body + the guard conditions at the top). I need to know:
1) Does it early-return when NOT in createMode? Quote the guard (e.g. `if (!createMode) return;` or `createRanRef.current`). If it early-returns for normal (non-create) viewing, it won't affect BHG normal selection — confirm.
2) The setSelectedMonthKey(mkParam) call — under what condition does it run? Only during create flow, or also on every enrichedSeries/rolling24 change in normal view?
3) createRanRef.current guard — does it prevent re-running after the first time? Quote it.
4) When the user is just VIEWING BHG (not creating a month) and selects 202605, will this effect fire (because enrichedSeries/rolling24 changed) and reset selectedMonthKey to mkParam? Yes/no, with quoted evidence.

OUTPUT:
- Full create-mode effect body + guards, quoted.
- Does it run in normal (non-create) view? yes/no.
- Will it reset the user's month selection on enrichedSeries/rolling24 change? yes/no + evidence.
- If YES it interferes: we need a guard so it does not overwrite an explicit user selection.

Just this. Then decide apply.
