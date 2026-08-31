Bug — "New Month" (Add New Month) value changed based on the
selected month instead of staying at latest + 1

STATUS: Resolved.

ROOT CAUSE: The "New Month" value was computed from the currently
selected month + 1, so selecting an earlier month (e.g. 202603)
changed New Month to 202604 instead of keeping it at the latest
existing month + 1.

FIX: "New Month" now derives from the latest existing month
(maxMonthKey) + 1, independent of the selected month — matching
legacy behavior. The month increment correctly handles the
December→January year rollover.

VERIFIED: BHG (latest 202605) shows New Month 202606 constantly
regardless of the selected month; B W I (latest 202604) shows
202605. Matches legacy, where New Month stays at latest + 1.
