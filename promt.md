Checklist Questionnaire report (Selection_id 10) — no visual prototype exists from Geoff. Investigate what already exists to determine the report's expected layout and data shape from the codebase itself, not guesswork. READ-ONLY, no edits. One pass, answer, STOP.

1. Open ChecklistQuestionnairePDF.tsx fully. Does it already have a defined layout (header, table columns, sections)? Paste its structure — what columns/fields does it render today (even though the data is currently empty)? This tells us the INTENDED design, since someone built this component against a real spec.

2. Open ChecklistQuestionnaireModels.cs (the DTO). What fields does the response shape carry (client info block, question rows — what properties exactly)? 

3. Find the Review Form's "Checklist" section (frontend/src/app/review/[ecif]/review-info/components/sections/ChecklistSection.tsx or similar) — this is where users enter checklist answers today. What questions/fields/structure does THAT UI have? The report should presumably reflect this same data.

4. Find the backend table(s) that store checklist answers (grep for "Checklist" in SqlReviewRepository.cs or similar) — what columns exist (question text, answer/response, comments, category)?

5. Check discovery/ folder or any legacy Access query references for "Checklist Questionnaire" — is there a legacy SQL query or Access report definition anywhere in the repo (even if just referenced by name) that shows the original report's shape?

6. Confirm: does completing the backend query need any NEW frontend PDF changes, or does ChecklistQuestionnairePDF.tsx already render whatever the DTO provides (meaning backend-only work completes this report)?

Report the PDF component's current layout, the DTO shape, the Review Form Checklist section's structure, and the backend storage columns. This gives us the report's design without needing a Geoff-provided prototype. Do NOT propose or write a fix yet.
