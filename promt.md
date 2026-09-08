I need you to fix the CRM Policy Exceptions report totals/exposure aggregation to match the prototype exactly.

IMPORTANT: Do not make assumptions and do not modify unrelated report logic. First inspect the existing backend repository/service/DTO and frontend PDF/report aggregation flow, identify where POLICY EXCEPTION TOTALS COUNT and EXPOSURE are calculated, and then implement the minimum correct fix.

## BUSINESS RULE — THIS IS THE REQUIRED BEHAVIOR

The prototype establishes the following aggregation rules:

### Rule 1 — Per Exception Description

For every unique Exception Description:

- COUNT = number of UNIQUE borrowers having that exception.
- EXPOSURE = SUM of the borrower's COMMITMENT, counted only once per unique borrower for that Exception Description.

A borrower must NOT be counted twice for the same Exception Description even if duplicate rows exist.

Example:

Borrower:
B & G FOOD ENTERPRISES LLC (Review ID 21748)
Commitment = $34,194,930

Exceptions:
- E17 - Real Estate - Minimum Equity
- E08 - Speculative Land Loan

Expected totals:

E08 - Speculative Land Loan
COUNT = 1
EXPOSURE = $34,194,930

E17 - Real Estate - Minimum Equity
COUNT = 1
EXPOSURE = $34,194,930


## Rule 2 — OVERALL TOTALS

The overall "Totals (N borrowers)" row must NOT sum the exposure values from the exception-description rows.

Instead:

- N borrowers = COUNT of UNIQUE borrowers across the entire report.
- Total Exposure = SUM of COMMITMENT for each UNIQUE borrower across the entire report.

This is critical because the same borrower can have multiple exception descriptions.

For example:

B & G FOOD ENTERPRISES LLC has:

E08 = $34,194,930
E17 = $34,194,930

There is only ONE unique borrower.

Therefore:

Totals (1 borrower)
COUNT = 2
EXPOSURE = $34,194,930

NOT:

$34,194,930 + $34,194,930 = $68,389,860

The prototype confirms that the correct overall exposure is $34,194,930.


## Rule 3 — Same Exception Description Across Multiple Borrowers

If the same Exception Description exists for multiple unique borrowers:

COUNT = number of unique borrowers
EXPOSURE = SUM of each unique borrower's commitment.

Prototype example:

E43 - Minimum Pre-Owner's Fixed Charge Coverage

Borrower 1:
CLEARPATH HOLDINGS LLC
Commitment = $11,806,735

Borrower 2:
LENNY'S MIDCO LLC
Commitment = $54,929,465

Expected:

E43
COUNT = 2
EXPOSURE = $66,736,200

Because:

$11,806,735 + $54,929,465 = $66,736,200


E44 - Minimum Post-Owner's Fixed Charge Coverage

Borrower:
SOUTHERN BREW CORP
Commitment = $23,395,641

Expected:

E44
COUNT = 1
EXPOSURE = $23,395,641


Overall:

Totals (3 borrowers)
COUNT = 3
EXPOSURE = $90,131,841

Because the three unique borrowers are:

CLEARPATH HOLDINGS LLC = $11,806,735
LENNY'S MIDCO LLC = $54,929,465
SOUTHERN BREW CORP = $23,395,641

Total:

$11,806,735 + $54,929,465 + $23,395,641
= $90,131,841


## VERY IMPORTANT — DO NOT DOUBLE COUNT

Do NOT calculate overall exposure like this:

SUM(perExceptionDescriptionExposure)

That is WRONG.

For example:

Borrower A:
Exception 1 = $10M
Exception 2 = $10M

Overall exposure must be $10M, not $20M.

The commitment belongs to the borrower, not separately to every exception.

Likewise, do not simply SUM every detail-row commitment because duplicate exception rows can exist.

The aggregation must operate on UNIQUE BORROWER + EXCEPTION DESCRIPTION for per-exception totals, and UNIQUE BORROWER for overall exposure.


## DEDUPLICATION RULE

Before calculating totals, determine the correct borrower identity from the existing data model.

Prefer the stable borrower/review identifier already used by the application, such as Review ID/customer identity, rather than relying only on display name if the existing model provides a reliable unique identifier.

For per-exception aggregation:

UNIQUE KEY:
Borrower identity + Exception Description

For overall aggregation:

UNIQUE KEY:
Borrower identity

Do not accidentally treat two duplicate database/detail rows for the same borrower and same exception as two borrowers.


## DETAIL SECTION

Do not unnecessarily change the Policy Exception Details section.

The details should continue to show the actual exception records according to the existing report requirements.

The fix is primarily for the POLICY EXCEPTION TOTALS aggregation and overall exposure calculation.

However, if the current detail data itself contains duplicate rows that are clearly the same borrower + same exception and the prototype requires them to be unique, investigate that separately before changing it.

Do not remove legitimate records.


## COMMITMENT VS EXPOSURE

Use the borrower's COMMITMENT value for this report.

Do NOT use:

- account balance
- exposure balance
- sum of balances
- exception count multiplied by commitment

The prototype's detail column is COMMITMENT and the totals column is EXPOSURE, where exposure is derived from the unique borrower's commitment.

Verify the existing data source/query and use the correct commitment field already available in the application.


## REQUIRED VALIDATION CASES

After implementing the fix, explicitly test these cases.

### Test Case 1 — One borrower, two different exceptions

Borrower A = $34,194,930

Exception A
Exception B

Expected:

Exception A: COUNT 1, EXPOSURE $34,194,930
Exception B: COUNT 1, EXPOSURE $34,194,930
Overall: 1 borrower, COUNT 2, EXPOSURE $34,194,930

The old incorrect result would be $68,389,860.
That must NOT happen.


### Test Case 2 — Same exception, two borrowers

Borrower A = $11,806,735
Borrower B = $54,929,465

Same Exception Description.

Expected:

COUNT = 2
EXPOSURE = $66,736,200


### Test Case 3 — Two exception types, three borrowers

E43:
Borrower A = $11,806,735
Borrower B = $54,929,465

E44:
Borrower C = $23,395,641

Expected:

E43 COUNT = 2
E43 EXPOSURE = $66,736,200

E44 COUNT = 1
E44 EXPOSURE = $23,395,641

Overall:
3 unique borrowers
COUNT = 3
EXPOSURE = $90,131,841


### Test Case 4 — Same borrower appears under multiple exceptions

Borrower A = $50,000,000

Exception A
Exception B
Exception C

Expected overall:

Borrowers = 1
COUNT = 3
Exposure = $50,000,000

NOT $150,000,000.


### Test Case 5 — Duplicate database rows

Borrower A = $50,000,000
Exception A appears twice due to duplicate/detail records.

Expected:

Exception A COUNT = 1
Exception A EXPOSURE = $50,000,000

Overall:

Borrowers = 1
Exposure = $50,000,000


## IMPLEMENTATION APPROACH

First trace:

1. Report API endpoint.
2. Repository/query producing Policy Exceptions.
3. DTO/model carrying borrower, exception description and commitment.
4. Any backend aggregation/grouping.
5. Any frontend aggregation/grouping.
6. PDF generation code.
7. Identify whether totals are currently calculated server-side or client-side.
8. Find exactly where exposure is currently being summed.

Then implement the fix at the appropriate single source of truth.

Do NOT create two competing aggregation implementations in backend and frontend.

If backend already returns totals, fix the backend aggregation and make frontend consume those totals.

If frontend currently calculates totals from details and backend does not return totals, fix the existing frontend aggregation instead.

Use the existing architecture and naming conventions.

## IMPORTANT SAFETY REQUIREMENTS

- Do not change unrelated reports.
- Do not change Policy Exception filtering logic.
- Do not change exception identification logic.
- Do not change the meaning of IDENTIFIED.
- Do not change detail-row ordering unless required.
- Do not hardcode the prototype values.
- Do not use customer name alone as the unique key if a stable borrower/review ID exists.
- Do not sum per-exception exposure values to calculate overall exposure.
- Do not multiply commitment by exception count.
- Do not introduce duplicate API calls.
- Do not rewrite the entire report if a focused aggregation fix is sufficient.

## ACCEPTANCE CRITERIA

The final report must match the prototype behavior:

POLICY EXCEPTION TOTALS:

Exception Description | COUNT | EXPOSURE

Each exception description:
COUNT = unique borrowers with that exception
EXPOSURE = commitment summed once per unique borrower for that exception

Totals:
COUNT = total exception occurrences after the required unique borrower+exception deduplication
"X borrowers" = unique borrowers across the whole report
EXPOSURE = commitment summed once per unique borrower across the entire report

Most importantly:

If one borrower has multiple exception descriptions, their commitment is shown in each applicable exception-description row, but it is counted ONLY ONCE in the overall exposure.

After making the change:

1. Show me the exact files changed.
2. Show me the exact aggregation logic before vs after.
3. Explain why the previous implementation double-counted the borrower exposure.
4. Run the relevant unit/tests/build.
5. Report the test results.
6. Verify the three prototype examples above numerically.
7. Do not commit anything unless explicitly asked.
