Bug 213 fix — Policy Exceptions library should sort by Code ALONE (currently sorts Category → Level → Code). SINGLE FILE, frontend. Show diff before applying.

FILE: frontend/src/app/maintenance/policy-exceptions/page.tsx

1. The sort comparator (lines ~180-184) currently sorts by category, then level, then code. Change it to sort by CODE ALONE:
   .sort((a, b) => (a.code ?? "").localeCompare(b.code ?? "", undefined, { numeric: true }))
   Use { numeric: true } so alphanumeric codes sort naturally (e.g. E2 before E10, not E10 before E2). Remove the category and level comparisons.

2. After a successful create (lines ~471-473), the new row is appended to the end without re-sorting, so it appears out of order until reload. Re-sort the rows array by code (same comparator as above) after inserting the new row, so a newly added code lands in its correct sorted position.

Do NOT change filtering, paging, the category filter, or the backend. Optionally the backend ORDER BY could be aligned to Exception_code for consistency, but it's not required since the frontend re-sorts — leave backend unchanged unless trivial.

List every line changed. Commit: "Fix Bug 213: Policy Exceptions library sorts by Code alone (numeric-aware), including after create".
