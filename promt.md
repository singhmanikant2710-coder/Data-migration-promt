SELECT COLUMN_NAME, DATA_TYPE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND (COLUMN_NAME LIKE 'Risk_recognition%'
    OR COLUMN_NAME LIKE 'Scorecard_mgmt%'
    OR COLUMN_NAME LIKE 'Underwriting%'
    OR COLUMN_NAME LIKE 'Credit_servicing%'
    OR COLUMN_NAME LIKE 'Loan_admin%')
ORDER BY COLUMN_NAME;





READ-ONLY. Do NOT edit. Report only. Use the LIVE database, ignore any columns.csv 
discovery file (it is outdated).

The live DB (02_CORE_02_Reviews) has UNSAT boolean columns per component 
(Risk_recognition_UNSAT bit, Scorecard_mgmt_UNSAT bit, Underwriting_UNSAT bit, 
Credit_servicing_UNSAT bit, Loan_admin_UNSAT bit) plus *_comments columns. 
The old *_rating columns no longer exist.

Report ONLY, from the frontend CRM Ratings tab component:
1. Does it render UNSAT checkboxes (boolean) + a comment box per component, or old 
   rating dropdowns? Show the JSX.
2. On save, what does it put in the payload for ratings — UNSAT booleans + comments, 
   or rating strings (riskRecognition etc.)? Show the setSection/payload calls.
3. The exact state field names for each of the 5 components (the UNSAT bool + comment).

Report only with exact code. No edits.


-------------CPAT ----------


 
Analyze this entire codebase and give me a structured summary with these sections:

1. PROJECT STRUCTURE: List all projects/folders in this solution and their 
   responsibilities (API, Application, Domain, Infrastructure, etc.)

2. ARCHITECTURE FLOW: Explain how a typical request flows through the layers 
   (Controller → Service → Repository → Database), with actual file/class names.

3. ENTITIES & DATABASE: List all entity models, their key properties, and 
   relationships between them (one-to-many, many-to-many, etc.)

4. API ENDPOINTS: List all controller endpoints with HTTP method, route, and a 
   one-line description of what each does.

5. BUSINESS LOGIC: For each major service/module, summarize in plain English what 
   business rules or validations it enforces.

6. DESIGN PATTERNS & DEPENDENCIES: Identify design patterns used (Repository, Unit 
   of Work, CQRS, etc.) and list key NuGet packages.

Do not modify any code. Just read and summarize everything clearly under these 
headings so I can explain this application to a client.
