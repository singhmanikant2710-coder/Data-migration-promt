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
