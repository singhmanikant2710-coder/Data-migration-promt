READ-ONLY. Confirm what getMaxMonthKey returns — is it the overall latest strMonthKey for the customer, or calendar-year-scoped?

CONTEXT: We're changing New Month to be based on maxMonthKey + 1 (latest existing month). For this to be correct for ALL customers (including non-December fiscal years like BHG/Van Zyverden), maxMonthKey must be the OVERALL latest month (e.g. 202605), not a calendar-year-scoped max (which would wrongly give 202512 for BHG).

1) Find the backend for getMaxMonthKey (search "max-monthkey" or getMaxMonthKey endpoint → repo method). Quote its SQL. Is it MAX(strMonthKey) overall for the customer, or filtered by calendar year / something?
2) For BHG (months up to 202605), would maxMonthKey return 202605 (overall max, correct) or 202512 (calendar max, wrong)?

OUTPUT:
- A) getMaxMonthKey backend SQL, quoted.
- B) Does it return the overall latest month, or a scoped max? For BHG would it be 202605 or 202512?
- C) If it's overall MAX(strMonthKey), the New Month fix is safe for all customers. If it's scoped, we need to note that.
- No fix. Findings only.
