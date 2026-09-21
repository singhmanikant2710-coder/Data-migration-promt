Important finding: querying [03_LIBRARY_10_Distribution Parties] for
"WAGNER, JOHN C" returns a row where the "Recipient_role" column contains the
value "17436" — which is exactly the same Employee ID used in the RM/PM
dropdown (Data Mart Trial source). This suggests Recipient_role may actually
store the employee ID for some/all rows, not a role-type string like
"PML"/"ECO"/"SCO" as we previously assumed when removing the Recipient_role
filter from the PML/ECO/SCO dropdowns (per the earlier "remove this condition"
task).

Before changing the email-matching logic, investigate:
1. Run: SELECT DISTINCT Recipient_role, COUNT(*) FROM
   dbo.[03_LIBRARY_10_Distribution Parties] GROUP BY Recipient_role ORDER BY
   COUNT(*) DESC — send me the result. I need to see whether Recipient_role
   consistently holds numeric employee IDs across all rows, or whether it's a
   mixed column (numeric IDs for some rows, role-type strings like "PML" for
   others).
2. Also confirm: for the PML/ECO/SCO dropdown fix we did earlier (removing the
   Recipient_role filter condition), what was that filter actually comparing
   Recipient_role against? Pull that code/query and tell me — I want to confirm
   we didn't misinterpret a role-type filter as something else, or vice versa.

Once we know what Recipient_role actually contains:
- If it's consistently the employee ID (or numeric IDs make up the vast majority
  of rows), switch the RM/PM email-resolution match to join on employee ID
  (Recipient_role = the ID from the dropdown) instead of name-string matching.
  This is far more reliable than any name-format normalization — drop the
  name-based WHERE clause entirely for this lookup.
- If it's a mixed/overloaded column, tell me what you find before I decide how
  to proceed — don't guess and don't silently change the PML/ECO/SCO dropdown
  behavior we already shipped based on this new information.

Do not touch the already-completed PML/ECO/SCO dropdown/search/save work unless
this investigation shows it's actually broken by a misunderstanding of
Recipient_role — flag that to me explicitly rather than fixing it silently.
