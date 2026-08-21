Hi Geoff,

Two things — the reports status you asked about, and an update on #184.

=== REPORTS STATUS ===

I verified each report against the current codebase. The following are fully built and wired to run from the Reports page dropdown:
• CRM Findings Summary Table
• CRM Summary
• CRM Findings and Observations
• Unsatisfactory Transactional Ratings
• Scorecard Results
• PD Grade Migration
• Policy Exceptions
• Non-Compliant Covenants
• CRO Production
• Final Memos
• Checklist Questionnaire

On Checklist Questionnaire — this one is actually already wired up on our side (the report component and its data call exist and run). The output depends on the backend query returning data, so if you'd like to align it with your prototype query/design, we can — but the report itself isn't missing.

CRM Findings Only is the one still in discussion (covered below).

1) Renaming "PD/LGD Grade Migration" to "PD Grade Migration": This is safe — it will NOT affect the report running. The report is matched internally by a stable identifier (it keys off "migration" in the name), not the exact display label, so renaming it in your Reports library will still route correctly. You can rename it without breaking anything.

2) CRM Findings Only — extending Findings-only to other reports: Yes, this approach works. Rather than a separate report, we can add a "Findings only" filter/toggle on the Reports page that runs the existing CRM Findings and Observations report filtered to findings. The same pattern could extend to other reports like CRM Summary where a similar sub-filter makes sense — though I'd scope each one individually, since what "findings only" means may differ per report. Suggest we start with CRM Findings and Observations, then identify which others would genuinely benefit.

=== #184 — Observations & Findings Excel Export (00-CRM Admin) ===

Thanks for the clarification — that reframes it for me. My earlier note treated this as default-value/data-migration, but based on your explanation this is about the export producing null in columns L, M, N, O, P (CRM Component, Code, Severity, Category, Description) when "00-CRM Admin / No CRM Findings" is selected, and those needing to populate with their respective values on save.

I'm investigating now to pin down whether the values exist on the saved record but the export isn't mapping them (export-side fix on us), or whether they're being stored as null on save (data-side). I'll confirm which it is and the fix path shortly — will update you once I've traced it end to end.

Happy to walk through any of the above.

Thanks,
Manikant
