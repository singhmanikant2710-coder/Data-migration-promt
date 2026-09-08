Bug 216 fix — CRM Scorecard Results details rows split across page boundaries, causing the top record on page 2 to lose its Customer Name cell and shift all columns left. Fix by preventing row splitting. SINGLE FILE. Show diff, do NOT commit.

FILE: frontend/src/components/pdf/ScorecardResultsPDF.tsx

1. Line ~414 (details data-row <View> in the .map over g.rows): add wrap={false} so each data row stays intact and moves whole to the next page instead of splitting. Match the existing pattern already used on the totals rows (lines 379/385) in this same file.

2. Line ~401 (details header row <View>): add wrap={false} too, so the header row can't split either.

3. Optionally (recommended, matches other CRM reports that span pages): add `fixed` to the details header row so it repeats at the top of each page. This is a UX improvement since the table now spans multiple pages — but if you want to keep the change minimal, do only steps 1-2 and skip this. Include it as a separate clearly-marked line so I can decide.

Do NOT change column widths, the overflow:hidden on styles.table, the data, or anything else. Only wrap={false} on the two rows (+ optional fixed header). 
Show diff. Rebuild. Do NOT commit.
