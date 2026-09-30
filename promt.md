Decisions:

1) Option B: show as stored, pad the dropdown too.
- Pad the RM/PM dropdown option values/labels to 5 characters in SQL
  (e.g. RIGHT('00000' + LTRIM(RTRIM(x)), 5)), so the stored "08784" matches
  the option "08784 - TERENCE J DOLCH". No duplicate/synthetic entries.
- Apply padding ONLY to numeric, non-empty IDs. NULL/blank/non-numeric must
  not become "00000".
- Matching/joins must stay on TRY_CONVERT(int, ...), unchanged.
- Note: in the next task the dropdown source will move from Data Mart Trial
  to Distribution Parties (already padded), so keep the padding logic in one
  shared place that's easy to reuse or remove.

2) Option A: name only.
- Treat '0' / '00000' (any all-zero value) as "no ID". Render the name alone
  if a name exists, and fully empty if the name is NULL too. Generic rule,
  applies to both RM and PM.

Report ADDED/REMOVED (file:line), the on-screen change for Customer Info
RM/PM dropdowns + Distribution Parties screen + reports, the NULL /
all-zero case, and the backend + frontend build results. Do not commit.
