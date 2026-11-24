UAT #258 – create Dev TEST upload files (no code changes)

1. READ-ONLY: find the exact upload format the Load Samples grid upload (↥ icon) expects – CSV and XLSX parser in frontend/src/app/load-samples/page.tsx (and any template/download icon): required headers, column order, which columns are mandatory (e.g. Customer #, Review Type, CRO, Hold), allowed values, date formats. Report it with file:line.

2. READ-ONLY SQL on Dev (CAS_RISKREVIEW_DEV): pick real customers that will PASS validation:
   - CUST_NUM present in dbo.[01_DATA_01_Data Mart Trial] with at least one account row,
   - not already a review in ANY sample (dbo.[02_CORE_02_Reviews].Customer_number) – so #229 hold does not trigger,
   - distinct, non-blank, no leading/trailing spaces.
   Use valid values for every other mandatory column (e.g. a real CRO name from dbo.[03_LIBRARY_05_CAS Users], a valid Review Type from Selections).

3. Create these files in test-data/uat258/ (this folder is never committed):
   - uat258-30.xlsx  – 30 customers
   - uat258-75.xlsx  – 75 customers
   - uat258-76.xlsx  – 76 customers
   - uat258-30.csv   – same 30 customers as CSV
   Use different customers in each file where possible so each can be loaded into its own test sample.

4. Do NOT change any source code, database data or Selections. Do not commit or push.

Report in chat: the expected format (headers), the SQL used, the file paths, and the customer count per file.
