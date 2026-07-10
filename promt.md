Read-only diagnostic, no edits to files. I want to inspect runtime values via the browser console instead.

Give me a set of console.log statements I can paste directly into the browser DevTools Console (not into the code) — or tell me exactly what to inspect — to check, for the currently rendered CRM Findings page:

1. The exact value of a finding row's `component` string (row.component) for the row showing "04-Credit Servicing".
2. Whether labelOpts (FINDING_LABELS) has a key exactly equal to that row.component string, and if so, how many entries labelOpts[row.component] contains.
3. The keys present in labelOpts at this moment (to see if "04-Credit Servicing" is among them or if the key differs by casing/whitespace/prefix).

If these values can't be reached from the console because they're inside the component closure, instead add a TEMPORARY single console.log inside CrmFindingsAndRatingsSection.tsx that logs { rowComponent: row.component, labelKeys: Object.keys(labelOpts), labelForRow: labelOpts[row.component] } right before the codeOptions.map render, so I can read it in the console. Mark it clearly as temporary debug to remove later.

STOP and ask before editing. Recommend which approach.
