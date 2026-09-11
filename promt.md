READ-ONLY. Find how to carry the Principal/Gross selector to the recompute so 60+ DPD %, Cash Collections %, and Net C/O % use the correct basis, and so the panel dropdown selection reflects in the Top Strip. Quote with paths.

ROOT CAUSE (confirmed): The tblMainCalcs recompute loop runs per60DPD/perCashCollections/perNetChargeOff over a numeric-only values bag. The selector string (strPrincipalOrGrossCalculationSelectionper60DPD / ...CashCollection / ...NetChargeOff) never survives the DTO, so sel="" → the calc takes the Gross branch and CLOBBERS the server's correct Principal-basis value. Also, the panel's Principal/Gross <Select> (principalGrossByLabel state) drives computeConsumerFinancePercentOverride for middle-panel tiles but NOT the Top Strip.

USER REQUIREMENT: When the user selects Principal (or Gross) in the panel dropdown for a metric, the Top Strip's same metric % must use that SAME basis (consistent).

1) Quote the recompute loop (edit/page.tsx ~L1253) and confirm it clobbers per60DPD/perCashCollections/perNetChargeOff with a wrong-basis value when the selector string is absent.
2) Quote the three selector strings' names and how principalGrossByLabel maps labels → selection (Principal N/R / Gross N/R). How does the panel dropdown set them?
3) Quote computeConsumerFinancePercentOverride and how it resolves the basis from principalGrossByLabel + uses the correct denominators (prior-month for Cash Coll, current for 60+DPD, Avg-TTM for Net C/O). This already works for middle-panel tiles.
4) Quote the Top Strip render for 60+ DPD %, Cash Collections %, Net C/O % (monthSummaryRegistry) — currently they do pickExact (server value) then fallback to the calc. The clobbered value is what pickExact finds.
5) FIX OPTIONS — evaluate:
   (a) Hydrate the selector strings onto the row (baseValues/merged) from principalGrossByLabel state before the recompute loop runs, so per60DPD/perCashCollections/perNetChargeOff take the correct branch. This makes both Top Strip and panel consistent with the dropdown.
   (b) Stop the clobber: in the loop, skip overwriting per* when the computed value is 0 or when a non-zero server value exists (guard). This restores the server's correct basis but doesn't make the Top Strip follow the dropdown live.
   (c) Apply computeConsumerFinancePercentOverride to the Top Strip render too (like middle-panel), so the dropdown selection drives the Top Strip %.
   Which option (or combination) makes the Top Strip follow the panel dropdown selection (user's requirement) with lowest risk and no regression to read-only View/Report pages?

OUTPUT:
- A) The clobbering loop, quoted.
- B) Selector strings + principalGrossByLabel mapping + how dropdown sets them, quoted.
- C) computeConsumerFinancePercentOverride (basis resolution + denominators), quoted.
- D) Top Strip render for the three metrics, quoted.
- E) Recommended fix: which option(s) make Top Strip follow the panel dropdown for all three metrics, generic across customers, without breaking View/Report pages. Exact location(s).
- No fix. Findings only.
