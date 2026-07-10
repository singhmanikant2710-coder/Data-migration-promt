Read-only diagnostic, no edits.

I need to trace codeOpts vs the label source on the render side of CrmFindingsAndRatingsSection.tsx.

Show me:
1. Exactly what `codeOpts` and `labelOpts` are assigned from — i.e. which hook return values feed them. Paste the lines like `const { FINDING_CODE_OPTIONS: codeOpts, FINDING_LABELS: labelOpts } = useCrmFindings(...)` or however they're destructured/aliased.
2. Confirm: is `codeOpts[row.component]` the SAME source (codeMap from lookups.findingCodes) that the label-building effect iterates over? Or does the dropdown get its list of codes from a DIFFERENT source (e.g. the full findings/library response, all CS-101..CS-116OLD) than codeMap?
3. For a given component like "04-Credit Servicing", print (or describe) whether codeOpts["04-Credit Servicing"] and the keys of labelOpts["04-Credit Servicing"] contain the SAME set of codes, or different sets.

This will tell me if the dropdown lists codes that were never in codeMap, so no label was ever built for them. Report only. STOP before editing.
