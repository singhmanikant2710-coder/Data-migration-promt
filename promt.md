Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #231, Geoff): Review Form → Covenants section ("Covenant and
Monitoring Information"). These three dropdowns must show GREEN text when
"Yes" and RED when "No":
1. Are monitoring covenants accurately tracked and defined
2. Are covenants accurately calculated and validated
3. Are covenant breaches adequately addressed and mitigated
Today they show "Yes" in red. "Is Stepped Up Servicing Required" is already
correct (Yes = red) — do NOT change it.
Any other value (N/A, blank/NULL, other) → neutral (same as today's
default/no colour).

Rules:
- Styling only. No change to values, options, save logic or data.
- Reuse the existing colour helper/classes the form already uses for
  Yes/No colouring (e.g. the one Stepped Up Servicing uses) — add a
  "positive polarity" option rather than duplicating logic. If the helper
  is shared with other sections, existing callers must keep their exact
  colours.
- Same colours in edit mode and read mode (disabled select).
- Do not change PDFs/reports.

PHASE 1 (read-only): file:line of the 4 Covenants dropdowns, the colour
logic they use today, and every other caller of that helper/class.
PHASE 2: smallest change.
PHASE 3: implement.
PHASE 4: build frontend.

Report:
1. What you found (file:line) + other callers of the helper
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after for each of the 4 fields (Yes / No / N/A /
   blank), edit + read mode
5. Edge cases: value casing/whitespace ("yes", "YES "), NULL, legacy values
6. Other sections/screens confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged; no harness files left)
Do not commit or push.
