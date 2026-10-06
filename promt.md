Hi Geoff,

I hope you're doing well. I have a small request.

For the release/1.0.0 changes to work fully in QA and Prod, the five database scripts below need to be executed. Could you please ask John to run them in QA and then Prod, in the order listed? Before running each one, it would be great if he could first check whether the table/row already exists in that environment, and only execute it if it is not already there. The scripts are in the repo under scripts/sql/.

1. create-distribution-parties-upload-objects.sql
   Supports the Admin > Distribution Parties Upload screen. Creates the staging table (04_TEMP_03_Distribution Parties) and the audit table (05_AUDIT_01_Distribution Parties Uploads) used to validate the uploaded file, preview it, replace the list in one transaction, and record each upload.
   If not run: the Distribution Parties Upload screen will fail.

2. insert-crm-summary-for-management-selection.sql
   Adds the "CRM Summary for Management" report to the Reports dropdown (Selections library).
   If not run: the report will not appear in the dropdown.

3. insert-crm-findings-for-management-selection.sql
   Adds the "CRM Findings for Management" report to the Reports dropdown.
   If not run: the report will not appear in the dropdown.

4. insert-samples-type-selections.sql (#226)
   Adds the Samples "Type" options (Continuous, Examination, CCL, Co-Source, Other) to the Selections library in the agreed order.
   If not run: the screen uses the old built-in list, so new options such as Co-Source will not be available.

5. insert-help-tip-checklist-questions.sql (#211)
   Adds the "Checklist Questions" help tip for the Review Form Checklist section (an exact copy of the QA row).
   If not run: the Checklist help icon will show "No help tip available."

All scripts only add new objects/rows – they do not update or delete existing data – and each ends with a verification query. I'd be grateful if John could share the output (or any errors) once done, and I'm happy to join a quick call if he has any questions.

Thank you so much for your help!

Best regards,
Manikant
