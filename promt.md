UAT #53 has been REOPENED by the client with three issues. Fix all three.

ISSUE 1 — Inactive codes still appear in the dropdown.
We added a filter for [Active] = 1 on dbo.[03_LIBRARY_01_CAS Findings], but inactive codes (e.g. CS-116OLD) are STILL showing. 
- Verify the filter is actually applied in the query that serves GET /api/v1/findings/library (SqlFindingsRepository.cs). 
- Check whether the frontend is calling a DIFFERENT endpoint or a cached source that bypasses the filter — the Finding Code dropdown options may come from response.lookups.findingCodes (the review payload), NOT from findings/library. If so, the Active filter must ALSO be applied to whatever query populates lookups.findingCodes.
- Report which query actually feeds the dropdown options, and apply the Active = 1 filter there too.

ISSUE 2 — The dropdown options are "widely dispersed" and rows are uneven.
Client: "Finding code drop-down options are widely dispersed. Previous edit allowed user to see Finding Code Description alongside Finding Code and all rows were even."
- In the SearchableSelect option rows, the description currently wraps to multiple lines, making row heights uneven and the list hard to scan.
- Fix: make every option row a UNIFORM height. Code in a fixed-width left column; description in the right column truncated to a SINGLE line with ellipsis (no wrapping). Add a title/tooltip attribute with the full description so hovering still reveals it.
- The result should look like the client's Access prototype: two clean columns, all rows the same height.

ISSUE 3 — The closed control shows the full "CODE - Description" label.
Client: "When selected and Saved, the Finding Code field shows the Finding Code and Description - We only need to see the Finding Code."
- The CLOSED SearchableSelect trigger must display ONLY the code (e.g. "CS-104"), never the description.
- The description must appear ONLY in the open dropdown list.
- Check the renderSelected prop — it should return just the code.

Constraints:
- Option value stays the raw finding code — save path unchanged.
- No new API calls.

Report what you find for Issue 1 first (which query feeds the dropdown), then apply all three fixes. Show me the diffs.
