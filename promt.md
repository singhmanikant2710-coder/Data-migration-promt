Test result: selecting the name-only option "JOHN C WAGNER II" returned 19
reviews, but it should return only the reviews with a NULL / all-zero ID
(max 4: 19035, 19358, 19587, 19841). The name predicate also matches
reviews that HAVE an ID (17436), so those reviews appear under both the ID
option and the name-only option.

Fix, generic across all 17 reports:
1. Add flags RelationshipManagerNameOnly / PortfolioManagerNameOnly (bool?)
   to ReportHierarchyFilters and the CRM PD Grade Migration request.
2. Frontend (managerRequestFields): when a name-only option
   (isEmployeeId = false, chosen from the grouped list) is selected, send
   the name AND NameOnly = true. Restored legacy plain-name state with no
   option match: send the name WITHOUT the flag (behaviour as today).
3. In every repository, next to the existing name predicate, add:
   AND (@RelMgrNameOnly IS NULL OR @RelMgrNameOnly = 0
        OR TRY_CONVERT(int, r.[Relationship_mgr_number]) IS NULL
        OR TRY_CONVERT(int, r.[Relationship_mgr_number]) = 0)
   (same for PM), plus the matching bindings. Mirror the existing
   @RelMgr / @RelMgrId counts exactly per file. Additive only, CRLF
   preserved. Verify with git diff --stat from the repo root.
4. No flag sent → every report returns identical rows to today.

Report ADDED/REMOVED (file:line), per-file predicate/binding count parity,
and the expected result for: ID option (23), name-only option (only the
NULL-ID reviews), no filter (unchanged), restored plain name (unchanged).
Backend + frontend build results. Do not commit.
