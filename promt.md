Please compare the attached CURRENT report screenshot with the attached PROTOTYPE screenshot.

The COUNT aggregation is now correct, but the TOTAL EXPOSURE aggregation is still incorrect.

CURRENT REPORT:
- E08 - Speculative Land Loan → COUNT 1 → EXPOSURE $34,194,930
- E17 - Real Estate - Minimum Equity → COUNT 1 → EXPOSURE $34,194,930
- TOTAL → COUNT 2 → EXPOSURE $34,194,930 ❌

PROTOTYPE:
- E44 - Minimum Post-Owner's Fixed Charge Coverage → COUNT 1 → EXPOSURE $23,395,641
- E43 - Minimum Pre-Owner's Fixed Charge Coverage → COUNT 2 → EXPOSURE $66,736,200
- TOTAL → COUNT 3 → EXPOSURE $90,131,841

The prototype makes the aggregation rule clear:
- Each exception description's EXPOSURE is the sum of the applicable detail-row borrower commitments.
- The grand TOTAL EXPOSURE is the sum of the exception-description exposure totals.
- It is NOT simply taking one review/borrower's exposure for the grand total.

For the CURRENT dataset:
$34,194,930 + $34,194,930 = $68,389,860

Therefore the expected current totals are:

COUNT = 2
EXPOSURE = $68,389,860

Please inspect the backend aggregation logic and fix the TOTAL EXPOSURE calculation so it sums the exposure values represented by the exception rows/categories, consistent with the prototype.

IMPORTANT:
- Do NOT change the already-correct exception descriptions.
- Do NOT change the already-correct COUNT = 2.
- Do NOT change the Policy Exception Details table.
- Do NOT change the commitment values in the details.
- Only fix the exposure aggregation so the totals table follows the prototype's calculation.

Also verify that the fix works generally, not only for this specific data.
