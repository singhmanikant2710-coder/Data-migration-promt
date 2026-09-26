Display fix (ATHENS, Monthly Summary): columns "Suppressed Availability"
and "Amzn $ Ineligible" show the number without "$"; legacy shows it
as currency.

1. READ-ONLY first: quote file:line where these columns are built and
   rendered, and where their format comes from (custom field format in
   tblCustomer / tblCustomerCustomTable, or kind hardcoded).
2. FIX: render them with the field's own format from the customer's
   custom field definition ("$" -> currency, same as legacy). Only if
   no format metadata exists, fall back to currency for these two
   labels. Do not change other custom fields or values.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is
