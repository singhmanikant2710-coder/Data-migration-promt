Fix the CRM Findings & Observations Excel Export issue under Reports without breaking or changing any existing functionality.

Bug:
The CRM Findings & Observations report displays records correctly, but the Excel export does not populate the required values for Review records where no CRM Finding/Observation exists. In the exported Excel, the CRM Finding-related columns are coming through as NULL/blank even though default values are required.

Please update ONLY the Excel export query/mapping for CRM Findings & Observations. Do not modify the existing report UI, CRM Findings/Observations creation or editing logic, existing filters, existing findings/observations data, or any other report/export functionality.

Required Excel Export changes:

1. CRM Component
   - Excel column: CRM Component
   - If there is no Finding/Observation record for the Review, populate:
     "00-CRM Admin"
   - This value should come from the corresponding Finding CRM Component/default mapping.

2. Code
   - Excel column: Code
   - If there is no Finding/Observation record for the Review, populate:
     "CRM-00"

3. Severity
   - Excel column: Severity
   - If there is no Finding/Observation record for the Review, populate:
     "N/A"
   - Existing historical records that already contain "Observation" or another valid Severity must NOT be overwritten.
   - "N/A" should be used as the default only when no CRM Finding/Observation exists.

4. Category
   - Excel column: Category
   - If there is no Finding/Observation record for the Review, populate:
     "No CRM Findings"

5. Description
   - Excel column: Description
   - If there is no Finding/Observation record for the Review, populate:
     "No CRM findings identified"

6. Balance
   - Add/populate the Balance column in the Excel export.
   - For each Review_id, calculate the SUM of [02_CORE_04_Accounts].Balance for all CORE_Accounts records where:
       CORE_Accounts.Review_id = [02_CORE_02_Reviews].Review_id
   - The Balance amount must be associated with the corresponding Review_id.
   - Do not duplicate/inflate the Balance amount because of joins with Findings/Observations.

7. Commitment
   - Add/populate the Commitment column in the Excel export.
   - For each Review_id, calculate the SUM of [02_CORE_04_Accounts].Commitment for all CORE_Accounts records where:
       CORE_Accounts.Review_id = [02_CORE_02_Reviews].Review_id
   - The Commitment amount must be associated with the corresponding Review_id.
   - Do not duplicate/inflate the Commitment amount because of joins with Findings/Observations.

Important:
- Preserve all existing Excel columns, column order, formatting, filters, and existing data behavior unless the above changes explicitly require otherwise.
- Do not change existing Finding/Observation records.
- Do not replace existing non-null CRM Finding values with the default values.
- Use conditional/default logic such as COALESCE/ISNULL (or the equivalent supported by the application's query implementation) only where appropriate.
- The default CRM-00 record values should appear only when the Review has no CRM Finding/Observation data.
- Ensure the export still returns all existing Review records, including Reviews with Findings, Reviews with Observations, and Reviews with no Findings/Observations.
- Prevent one-to-many joins from creating duplicate Excel rows.
- Balance and Commitment must be aggregated at Review_id level before/independently of the Findings/Observations join so that amounts are not multiplied by the number of findings/observations.

Expected result:

For a Review with an existing Finding/Observation:
    CRM Component = existing value
    Code          = existing value
    Severity      = existing value
    Category      = existing value
    Description   = existing value
    Balance       = SUM(Account.Balance for Review_id)
    Commitment    = SUM(Account.Commitment for Review_id)

For a Review with NO Finding/Observation:
    CRM Component = "00-CRM Admin"
    Code          = "CRM-00"
    Severity      = "N/A"
    Category      = "No CRM Findings"
    Description   = "No CRM findings identified"
    Balance       = SUM(Account.Balance for Review_id)
    Commitment    = SUM(Account.Commitment for Review_id)

Validation:
1. Test a Review that has CRM Findings.
2. Test a Review that has CRM Observations.
3. Test a Review that has no Findings/Observations.
4. Verify the five default CRM values appear correctly for the no-Finding case.
5. Verify existing Finding/Observation values remain unchanged.
6. Verify Balance equals the exact SUM of CORE_Accounts.Balance for that Review_id.
7. Verify Commitment equals the exact SUM of CORE_Accounts.Commitment for that Review_id.
8. Verify Balance and Commitment are not duplicated or multiplied when a Review has multiple Findings/Observations.
9. Verify the Excel export does not create duplicate rows.
10. Verify all existing Reports functionality continues to work exactly as before.

Make the smallest possible code/query change necessary to fix this issue and do not introduce any regression in existing functionality.
