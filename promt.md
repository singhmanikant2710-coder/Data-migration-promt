#185 — CRM Findings Only Report — Scoping Needed
Hi [Krishan/Brijesh],
#185 asks for a brand-new "CRM Findings Only" report (both PDF and Excel) — identical to CRM Findings and Observations but filtered to records where Severity = "Finding", selectable from the Reports Home page, with title/footer "CRM Findings".
This is new report functionality rather than a bug fix, so I wanted to align on scope before starting. A few points to confirm:
The ticket notes it should follow the corrections to #182 and #183 (both now addressed), so timing-wise it's ready to pick up.
Approach: whether to build a new PDF/Excel component or parameterize the existing CRM Findings and Observations report with a "Finding-only" filter (I'd lean toward reusing the existing report with a filter flag to avoid duplication).
Where the "Finding" filter should apply — backend query vs frontend filtering.
Adding the new report to the Reports Home dropdown/registry and its report ID mapping.
Happy to proceed once we've aligned on the approach. I can put together a short implementation plan if useful.
