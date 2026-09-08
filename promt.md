Please investigate and fix UAT Bug #214 for the CRM Findings Summary Table report.

I have attached:
1. The UAT issue screenshot showing the reported defect.
2. The CRM Findings Summary Table report screenshot showing the current output.

BUG:
In the CRM Findings Summary Table, under BORROWER FINDING TOTALS, the Credit Servicing row is double-counting exposure when the same borrower has multiple findings for the same CRM Component.

Current Credit Servicing values:
- COUNT = 5
- EXPOSURE = $223,630,000
- FINDING TOTALS COUNT = 6

The COUNT is correct and must remain 5.

The issue is the EXPOSURE calculation.

The Collier at Clift Farm LLC has:
- Exposure = $30,100,000
- 2 Credit Servicing findings

The current aggregation is counting this borrower's $30,100,000 exposure twice because the borrower has two findings.

Expected Credit Servicing exposure:
$223,630,000 - $30,100,000 = $193,530,000

Therefore the expected row should be:

Credit Servicing
COUNT = 5
EXPOSURE = $193,530,000

The FINDING TOTALS COUNT = 6 should remain unchanged because there are 6 findings across 5 unique borrowers.

IMPORTANT BUSINESS RULE:
For BORROWER FINDING TOTALS, COUNT and EXPOSURE must be calculated at the UNIQUE BORROWER/REVIEW level within each CRM Component, not at the individual finding-row level.

If a borrower has multiple findings for the same CRM Component:
- Count that borrower only once in BORROWER FINDING TOTALS.
- Include that borrower's exposure only once.
- Do NOT sum the same borrower's exposure once per finding.

However, FINDING TOTALS should continue to count individual findings, so the existing FINDING TOTALS COUNT should not be changed.

Please inspect the backend repository/query/aggregation logic that builds:
- UNSATISFACTORY TRANSACTIONS
- BORROWER FINDING TOTALS
- FINDING TOTALS

Identify exactly where the Credit Servicing exposure is being duplicated and fix the aggregation at the correct layer rather than hardcoding this specific borrower or amount.

Also verify that the fix works generally for any CRM Component where a borrower has multiple findings.

Expected result for this dataset:

Risk Recognition:
  Borrower Count = 2
  Borrower Exposure = $66,600,000

Scorecard Management:
  Borrower Count = 0
  Borrower Exposure = $0

Underwriting:
  Borrower Count = 2
  Borrower Exposure = $89,813,569

Credit Servicing:
  Borrower Count = 5
  Borrower Exposure = $193,530,000  <-- FIX REQUIRED

Loan Administration:
  Borrower Count = 2
  Exposure should remain based on unique borrowers

Do NOT change the finding counts or the FINDING TOTALS logic unless the investigation proves they are incorrect.

Also verify the percentage displayed for BORROWER FINDING TOTALS after correcting the exposure, because the percentage is calculated from the corrected exposure against the sample exposure.

Please provide the root cause and exact files/lines changed before considering the bug fixed.
