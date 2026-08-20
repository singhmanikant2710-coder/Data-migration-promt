READ-ONLY INVESTIGATION ONLY.
DO NOT MODIFY ANY FILE OR CODE.

We have now confirmed the following:

1. Chrome Network Save request contains:
   accuratelyDefinedTracked: "N/A"

2. The backend calls:
   SaveCovenantsInfoAsync(...)

3. In:
   backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

   SaveCovenantsInfoAsync directly executes:

   UPDATE dbo.[02_CORE_02_Reviews]
   SET
       [Covenant_tracking_accuracy] = @accDefinedTracked,
       [Covenant_validation_accuracy] = @accCalculated,
       [Covenant_breaches_addressed] = @breachesMitigated
   WHERE [Review_id] = @id

4. The parameters are NVARCHAR and directly receive:
   accuratelyDefinedTracked
   accuratelyCalculated
   breachesMitigated

There is NO N/A → No conversion in this repository method.

Therefore continue the investigation AFTER SqlReviewRepository.cs.

IMPORTANT:
Do NOT re-read frontend files.
Do NOT investigate CovenantsSection.tsx.
Do NOT modify anything.

Find the exact reason why the database/GET response changes "N/A" back to "No".

Investigate in this exact order:

1. Find the GET/reload API endpoint that returns the Covenant "info" object.

2. Find the exact repository method/query used by that GET endpoint.

3. Find where these database columns are mapped:
   - Covenant_tracking_accuracy
   - Covenant_validation_accuracy
   - Covenant_breaches_addressed

4. Check whether the GET query/mapping converts:
   "N/A" → "No"

5. Check the database definition of:
   dbo.[02_CORE_02_Reviews]

   Specifically determine the data type and constraints of:
   - Covenant_tracking_accuracy
   - Covenant_validation_accuracy
   - Covenant_breaches_addressed

6. Search the codebase/database scripts for:
   - triggers on dbo.[02_CORE_02_Reviews]
   - UPDATE statements against these three columns
   - CASE expressions
   - COALESCE / ISNULL
   - Yes/No normalization
   - "N/A"
   - "Covenant_tracking_accuracy"
   - "Covenant_validation_accuracy"
   - "Covenant_breaches_addressed"

7. IMPORTANT:
   Determine whether another save operation is overwriting N/A with No AFTER SaveCovenantsInfoAsync executes.

We need to establish this exact data flow:

"N/A" from browser
→ Controller
→ SaveCovenantsInfoAsync
→ SQL UPDATE
→ DATABASE VALUE
→ GET QUERY
→ API RESPONSE

For each step tell me the actual value:
N/A or No.

OUTPUT ONLY:

A. Save method confirmed: whether it preserves N/A
B. Exact GET endpoint
C. Exact GET repository method
D. Exact SQL/query used by GET
E. Exact DB columns
F. DB data type for each column
G. Any trigger/constraint/default/normalization found
H. Exact location where N/A becomes No
I. Exact file + method + line number
J. Minimal fix required

DO NOT MODIFY ANYTHING.
STOP after root cause is proven.
