Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCrmFindings.ts

Update BOTH the CrmComponentId type union AND the COMPONENT_OPTIONS array 
to exactly match the database's component list. The correct, complete list is:

  "00-CRM Admin"
  "01-Risk Recognition"
  "02-Scorecard Management"
  "03-Underwriting"
  "04-Credit Servicing"
  "05-Loan Administration"
  "06-Servicing Systems"
  "07-Data Integrity"

Specifically:
- Replace "06-Data Integrity" with "06-Servicing Systems"
- Add "00-CRM Admin" (currently missing)
- Add "07-Data Integrity" (currently missing)
- Keep all entries in this exact order and exact spelling.

Apply the same change to ALLOWED_COMPONENTS if it is defined in this same file.

Do NOT change any other file. If ALLOWED_COMPONENTS or this list lives in a 
different file too, STOP and tell me first — do not edit it.
