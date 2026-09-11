IMPORTANT: Do NOT modify any BCAT legacy formulas or fiscal-year calculation logic.

The legacy BCAT formulas are already implemented consistently for the fiscal-year logic. The formulas must remain unchanged.

The issue we need to fix is ONLY the initial Month selection/default Month state in the Black Book Edit UI.

Observed behavior:
- Customer: ADIR INTERNATIONAL LLC
- Database/latest-month query returns latest_month = 202412.
- In FY 2024, the UI correctly recognizes 202412.
- However, on the relevant initial load/state, the Month dropdown can initialize to the previous month (202411) even though 202412 is the customer's latest available month.
- Clicking "Add New Month" causes the UI to move to the correct latest month, which indicates the underlying month data is already available.

Required fix:
1. Keep all existing BCAT formulas exactly as they are.
2. Keep the existing fiscal-year logic exactly as it is.
3. Do not change any formula definitions, calculation fields, Summary/Detail calculations, YTD calculations, TTM calculations, or legacy formula behavior.
4. Fix only the logic responsible for initializing the selected Month when the page/customer data loads.
5. The selected Month must be derived dynamically from the customer's existing latest available month returned by the current data/query.
6. Do NOT hardcode 202412 or any other month.
7. Do NOT calculate the default month as simply "previous month".
8. Do NOT require the user to click "Add New Month" to get the correct month.
9. Do not add/remove any UI rows, dropdown options, or data rows.
10. Do not change the existing month list/order.
11. Do not change FY switching behavior.
12. Do not change the behavior of "Add New Month".
13. Make the smallest possible code change.

Before modifying code:
- Trace where latest_month/latest fiscal month is received.
- Trace where the initial selected Month state is set.
- Identify why the UI is initially choosing 202411 instead of the available latest month 202412.
- Pay particular attention to state initialization/useEffect execution order and whether the default month is being set before the customer/month data has finished loading.

Validation:
- Freshly open ADIR INTERNATIONAL LLC.
- Without clicking "Add New Month", verify the correct latest available month is selected automatically.
- Verify FY 2024 → 202412 remains correct.
- Verify FY 2025 behavior remains unchanged.
- Verify another customer with a different latest month also gets that customer's latest month dynamically.
- Verify all existing formulas and calculated values remain unchanged.
- Verify "Add New Month" still works exactly as before.

This is a targeted UI state/default-selection bug fix only. Do not refactor unrelated code and do not modify legacy BCAT formulas.
