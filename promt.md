Investigate current state of "Unsatisfactory Transactional Ratings" (Selection_id 5) — it now appears in the dropdown (confirmed via screenshot), but does clicking Generate actually produce a working report? READ-ONLY, no edits. One pass, answer everything, STOP.

1. In reports/page.tsx, does toReportId() have a rule matching "Unsatisfactory Transactional Ratings" (or "unsatisfactory transactional ratings")? What report ID does it resolve to?
2. In onGeneratePdf(), is there an is*() guard matching that report ID? Does it call a real PDF component, or does it fall through to the generic placeholder (the "PDF generation is not implemented for this report" toast we saw before for this exact report)?
3. Is there a PDF component file for it anywhere in frontend/src/components/pdf/ (even a stub)?
4. Backend: is there a model, repository, service, and controller endpoint for "unsatisfactory-transactional-ratings" (or similar route)? Search ReportsController.cs and StartupExtensions.cs for any registration.
5. If backend pieces exist, are they wired end-to-end (DI registered, controller endpoint reachable), or do dead/orphaned pieces exist that were never connected?

Report the EXACT current state — what exists, what's missing, and what happens today if a user selects this report and clicks Generate. Do NOT assume based on naming similarity to other reports; verify each layer independently.
