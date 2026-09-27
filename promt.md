Correction: legacy always shows custom field slots 1..4, with the header
taken from tblCustomer.strCustomFieldDescription{i} as-is — including
the default "Custom Field {i}" label (ATHENS slot 4 = "Custom Field 4"
is shown in legacy and in the edit page).

In report/page.tsx customerCustomColumns / applyCustomerColumnSet:
do NOT drop slots 1..4 because their label is "Custom Field {i}".
Keep hiding slots 5..10. Only a truly blank label may be hidden.
Build, do not commit.

REPORT FORMAT (mandatory):
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE incl. NULL value case
- NOT TOUCHED
