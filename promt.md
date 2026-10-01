Slice A verified (18 files, inert). Go ahead with Slice B.

Requirements:
1. New grouped-options endpoint for RM and PM returning { value, label }:
   - value = Employee ID as stored (padded string), label = "ID - NAME".
   - NAME = Distribution Parties canonical name if the ID exists there,
     else the most frequent stored variant (max Review_id tiebreak).
   - One option per numeric ID (TRY_CONVERT(int, ...)). Never merge
     different IDs, never merge by name similarity.
   - Legacy name-only rows (NULL / all-zero ID): separate options,
     value = exact trimmed name, label = name, sent as the existing name
     filter (not as an ID).
2. reports/page.tsx: an ID option sends RelationshipManagerId /
   PortfolioManagerId and leaves the name filter EMPTY; a name-only option
   sends the name and leaves the ID empty. Never send both for one selection.
3. "Applied Report Filters" in every PDF shows the selected option's label,
   never a raw ID.
4. No filter selected: request identical to today.
5. Don't change Customer Info dropdowns, sample-load, or any report layout.

Report ADDED/REMOVED (file:line), on-screen change in the RM/PM filter
dropdowns (Wagner example), what each option type sends in the request,
the NULL / all-zero / no-filter cases, and the build results. Do not
commit.
