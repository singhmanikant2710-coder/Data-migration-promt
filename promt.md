Approved with one condition — before you apply, confirm this explicitly:

The mergeValues fix in FormChangesContext changes behaviour for EVERY section that uses it (Customer Info, Covenants, Policy Exceptions, Collateral, etc.). Confirm that:
1. You have checked which other sections stage arrays or nested objects via setSection/setField.
2. The array-safe merge does NOT change how those sections behave — their existing save payloads must be byte-for-byte identical to today.
3. Specifically: for scalar/string fields (like Customer Info's setField), mergeValues must behave exactly as the current spread does — last write wins, no accidental deep-merge of values that should be replaced.

State this confirmation in your summary. Then apply the plan.

Show me the diff for FormChangesContext FIRST (the root fix), before the other three files. STOP after that diff for my approval.
