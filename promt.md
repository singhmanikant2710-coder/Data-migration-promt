Read-only diagnostic, no edits. Two things to determine.

OBSERVED BEHAVIOUR:
On the CRM Findings tab (in Edit mode), I add a row: Component = 01-Risk Recognition, Finding Code selected, Severity = Finding. WITHOUT saving, I switch to the CRM Ratings tab — the Risk Recognition Findings count shows 0. When I switch BACK to CRM Findings, my unsaved row is gone and the saved DB data is shown again.

INVESTIGATE AND REPORT:

1. Is the loss of unsaved edits on tab/section switch INTENTIONAL app behaviour, or a bug?
   - Where is FormChangesContext provided (which component)? Does that provider UNMOUNT when the user switches sections, discarding pending changes?
   - Do OTHER sections (e.g. Customer Info, Covenants) also lose unsaved edits when you switch tabs, or do they retain them? Check the code and tell me whether this is consistent app-wide.

2. Does `changes.changes.crmFindingsAndRatings.findings` still hold the pending rows at the moment the user lands on CRM Ratings, or is it already cleared? Prove it from the code.

Based on the answer, tell me which is true:
(A) Unsaved edits are SUPPOSED to survive tab switches, and this is a bug that needs fixing (in which case: what is the root cause and the minimal fix?), OR
(B) The app is designed so the user must Save before switching tabs, and pending changes are intentionally discarded (in which case UAT #57 simply needs the sub-totals to derive from SAVED findings, and the user must Save the CRM Findings tab first for the counts to update).

Report with evidence from the code. STOP before editing.
