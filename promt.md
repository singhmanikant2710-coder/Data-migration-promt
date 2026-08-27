READ-ONLY. Read once, quote, stop. No loop. 

computeConsumerFinancePercentOverride overrides percent tiles in ConsumerFinance with Avg-TTM-based denominators, making Cash Collections % (0.27% vs legacy 0.34%) and 60+ DPD % (0.19% vs legacy 0.17%) WRONG. The backend-stored perCashCollections is already correct (prior-month, legacy-match).

Quote:
1) The full computeConsumerFinancePercentOverride function. Does it branch by label? For which labels does it return a non-null override (Cash Collections %, 60+ DPD %, Net C/O %)?
2) The call site: `if (isConsumerFinance && kind==="percent") { const ov = computeConsumerFinancePercentOverride(...); if (ov !== null) effectiveVal = ov; }`. Quote it.
3) KEY QUESTION: if this override were removed/returned null for Cash Collections % and 60+ DPD %, would those tiles fall back to the correct backend-computed stored value (which is legacy-correct)? What is effectiveVal BEFORE the override is applied — is it the stored perCashCollections/per60DPD?

OUTPUT:
- A) The function + which labels it overrides, quoted.
- B) What effectiveVal is before the override (the stored correct value?), quoted.
- C) State: would returning null (skipping override) for Cash Collections % and 60+ DPD % restore the legacy-correct stored values?
- No fix. Findings only.
