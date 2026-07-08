Task: On the Customer Info tab, make the "CUSTOMER SIZE" field an editable dropdown (select) instead of a read-only text field.

Dropdown options (hardcoded, in this exact order):
<$1MM, $1MM-$5MM, $5MM-$20MM, $20MM-$50MM, $50MM-$100MM, >$100MM

Constraints:
- Use live DB knowledge only; ignore backend-schema/columns.csv.
- READ-ONLY diagnostics first: locate the Customer Info frontend component and show me the current "CUSTOMER SIZE" field JSX block before editing.
- Single-file edit only (frontend component). Do NOT change any DTO/API/backend in this step.
- Convert the field to a dropdown bound to the same value/name used now (so the selected value saves through the existing Save path). Keep the current value pre-selected on load.
- Show the diagnostic and your proposed edit, then wait for my confirmation before applying.
