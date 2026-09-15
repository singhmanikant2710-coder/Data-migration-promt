New reports scoping — 4 reports need to be built: "CRM Summary for Management" (185), "CRM Findings for Management" (208/217), "CRM Unsatisfactory Transactional Ratings" (219), "Checklist Questionnaire" (221). READ-ONLY, no edits. One pass, answer everything, STOP.

For EACH of these 4 report names, investigate:
1. Does it appear in the Reports dropdown (frontend catalog / 03_LIBRARY_09_Selections)? What is its exact current name in the dropdown?
2. Is there ANY existing routing logic in reports/page.tsx (onGenerate/onExport) that matches this report name — even partially? Does clicking "Generate" do anything (produce a PDF, show an error, do nothing, fall through to a placeholder)?
3. Is there an existing PDF component file for it (even a stub/incomplete one) under frontend/src/components/pdf/?
4. Is there an existing backend repository/service/controller endpoint for it (even a stub)?
5. Specifically confirm/deny Geoff's report: "Unsatisfactory Transactional Ratings" shows in the dropdown but PDF generation does nothing because there's no logic wired — trace exactly what happens today when it's selected and "Generate" is clicked.

Report, for each of the 4 reports: (a) dropdown status, (b) any existing frontend routing/PDF component, (c) any existing backend logic, (d) exact current behavior when a user tries to generate it. This tells us whether we're building from scratch or completing partial work. Do NOT propose or write a fix yet.
