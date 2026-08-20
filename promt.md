FIX REQUEST — Covenants N/A VALUE IS LOST AFTER SAVE + RELOAD

Issue:
In the Review Form → Covenants section, the three fields below support:
- Yes
- No
- N/A

When I select "N/A" and click Save, the save request payload correctly contains "N/A".

Example from Network → save request:
covenants.info.accuratelyDefinedTracked = "N/A"
covenants.info.accuratelyCalculated = "N/A"
covenants.info.breachesMitigated = "N/A"

However, after the page reloads, the GET review API returns "No" instead of "N/A".

This means the value is being lost/converted somewhere in the GET/read/response mapping path.

IMPORTANT:
The database/save side is already sending "N/A" correctly. Do NOT unnecessarily change the save logic or database logic.

Observed evidence:
1. Save request payload contains:
   accuratelyDefinedTracked: "N/A"
   accuratelyCalculated: "N/A"
   breachesMitigated: "N/A"

2. SQL repository SaveCovenantsInfoAsync currently accepts these as nullable strings:
   string? accuratelyDefinedTracked
   string? accuratelyCalculated
   string? breachesMitigated

3. SaveCovenantsInfoAsync updates:
   [Covenant_tracking_accuracy] = @accDefinedTracked
   [Covenant_validation_accuracy] = @accCalculated
   [Covenant_breaches_addressed] = @breachesMitigated

4. The GET/read side reads these database values using GetString():
   trackAcc = rdo0.IsDBNull(4) ? null : rdo0.GetString(4);
   calcAcc = rdo0.IsDBNull(5) ? null : rdo0.GetString(5);
   breaches = rdo0.IsDBNull(6) ? null : rdo0.GetString(6);

Therefore, "N/A" should be preserved.

ROOT CAUSE FOUND:
In the GET Covenants mapping there is a helper method:

   static string Yn(string? v)

Current behavior is effectively:

   var s = (v ?? string.Empty).Trim().ToLowerInvariant();

   if (s == "yes" || s == "y" || s == "true" || s == "1")
       return "Yes";

   if (s == "no" || s == "n" || s == "false" || s == "0")
       return "No";

   return "No";

The final:
   return "No";

is converting "N/A" into "No".

This is why:
DB = "N/A"
→ GetString() = "N/A"
→ Yn("N/A")
→ "No"
→ GET API returns "No"
→ UI shows "No" after reload.

REQUIRED FIX:
Modify ONLY the normalization/mapping logic so that "N/A" is preserved.

Expected mapping:

   "Yes" / "yes" / "Y" / "true" / "1" → "Yes"
   "No" / "no" / "N" / "false" / "0" → "No"
   "N/A" / "n/a" / "N/a" → "N/A"

For unknown/empty values, preserve the existing behavior unless there is a strong reason not to.

Most importantly:
- "N/A" MUST NOT become "No".
- Existing Yes/No behavior MUST remain exactly the same.
- Existing color logic MUST remain unchanged:
    Yes → red
    No → green
- Do not modify unrelated fields.
- Do not modify SUS Required logic.
- Do not modify dates.
- Do not modify narrative.
- Do not modify covenant rows.
- Do not modify transactions, checklist, policy exceptions, collateral, etc.
- Do not modify database schema.
- Do not create a new database migration.
- Do not refactor unrelated code.
- Do not change API contracts unnecessarily.

FILES / METHODS TO CHECK:

1. FRONTEND
File:
frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx

Relevant handlers:
- accuratelyDefinedTracked onChange
- accuratelyCalculated onChange
- breachesMitigated onChange

The frontend was already changed so that N/A is passed directly:

   const val = e.target.value as "Yes" | "No" | "N/A";
   setAccuratelyDefinedTracked(val);

   if (isEditing && changes) {
       changes.setField("covenants", "info", {
           accuratelyDefinedTracked: val
       });
   }

Similarly for:
- accuratelyCalculated
- breachesMitigated

DO NOT undo this frontend behavior.

The save payload must continue sending:
"N/A"

2. BACKEND CONTROLLER
File:
backend/src/Casrr.Api/Controllers/CovenantsController.cs

Relevant:
CovenantsController

Also inspect the ReviewController Save endpoint because the screenshot shows the main save flow eventually calls:

   _svc.SaveAsync(dto, emp, ct);

Do not change the controller unless absolutely necessary.

The controller should continue accepting the existing DTO and passing the Covenants data through.

3. BACKEND SERVICE
Review the service method responsible for saving the Review Form, visible in the ReviewController flow:

   _svc.SaveAsync(dto, emp, ct)

Relevant Covenants processing calls:

   _repo.SaveCovenantsInfoAsync(
       resolvedReviewId,
       lastAnnual,
       nextAnnual,
       susRequired,
       lastSusDate,
       accuratelyDefinedTracked,
       accuratelyCalculated,
       breachesMitigated,
       narrative,
       ct);

DO NOT remove or change the existing save behavior.

4. REPOSITORY INTERFACE
File:
backend/src/Casrr.Infrastructure/Interfaces/IReviewRepository.cs

Relevant method:

   Task SaveCovenantsInfoAsync(
       int reviewId,
       DateTime? lastAnnualReview,
       DateTime? nextAnnualReview,
       bool? steppedUpServicingRequired,
       DateTime? lastSusDate,
       string? accuratelyDefinedTracked,
       string? accuratelyCalculated,
       string? breachesMitigated,
       string? narrative,
       CancellationToken ct);

Do not change the public contract unless absolutely required.

5. SQL REPOSITORY — SAVE
File:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Relevant method around line 1099:

   public async Task SaveCovenantsInfoAsync(...)

Current SQL:

   UPDATE dbo.[02_CORE_02_Reviews]
   SET
       [Last_annual_review_date] = @lastAnnualReview,
       [Next_annual_review_date] = @nextAnnualReview,
       [Stepped_up_servicing] = @susRequired,
       [Last_SUS_date] = @lastSusDate,
       [Covenant_tracking_accuracy] = @accDefinedTracked,
       [Covenant_validation_accuracy] = @accCalculated,
       [Covenant_breaches_addressed] = @breachesMitigated,
       [Covenant_information] = @narrative
   WHERE [Review_id] = @id;

Current parameters use NVARCHAR for the three accuracy fields.

DO NOT change this save SQL unless investigation proves it is necessary.

6. SQL REPOSITORY — GET / READ
Same file:

backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Relevant method:

   private async Task<CovenantsSection> GetCovenantsSectionAsync(
       int? reviewId,
       CancellationToken ct)

This method reads:

   trackAcc = rdo0.IsDBNull(4) ? null : rdo0.GetString(4);
   calcAcc = rdo0.IsDBNull(5) ? null : rdo0.GetString(5);
   breaches = rdo0.IsDBNull(6) ? null : rdo0.GetString(6);

This proves the DB value can reach the C# variables as a string.

IMPORTANT:
There is also SUS parsing:

   var v = rdo0.GetValue(2);
   if (v is bool b)
       sus = b ? "Yes" : "No";
   else
       sus = Convert.ToString(v, CultureInfo.InvariantCulture);

DO NOT modify this SUS Required logic.
It is a separate field and is not the N/A issue.

7. EXACT ROOT-CAUSE AREA
Inside:

   GetCovenantsSectionAsync(...)

there is currently a helper similar to:

   static string Yn(string? v)
   {
       var s = (v ?? string.Empty).Trim().ToLowerInvariant();

       if (s == "yes" || s == "y" || s == "true" || s == "1")
           return "Yes";

       if (s == "no" || s == "n" || s == "false" || s == "0")
           return "No";

       return "No";
   }

This helper is used for the covenant accuracy/breach fields.

CHANGE ONLY THIS LOGIC.

Preferred safe implementation:

   static string Yn(string? v)
   {
       var s = (v ?? string.Empty).Trim();

       if (s.Equals("yes", StringComparison.OrdinalIgnoreCase) ||
           s.Equals("y", StringComparison.OrdinalIgnoreCase) ||
           s.Equals("true", StringComparison.OrdinalIgnoreCase) ||
           s == "1")
           return "Yes";

       if (s.Equals("no", StringComparison.OrdinalIgnoreCase) ||
           s.Equals("n", StringComparison.OrdinalIgnoreCase) ||
           s.Equals("false", StringComparison.OrdinalIgnoreCase) ||
           s == "0")
           return "No";

       if (s.Equals("n/a", StringComparison.OrdinalIgnoreCase))
           return "N/A";

       return "No";
   }

The key requirement is:
"N/A" must return "N/A".

If the existing codebase has a better equivalent style, you may use it, but keep the change minimal.

EXPECTED END-TO-END RESULT:

CASE 1:
User selects "Yes"
→ Save
→ DB = "Yes"
→ Reload
→ API = "Yes"
→ UI = "Yes"
→ Existing RED styling remains.

CASE 2:
User selects "No"
→ Save
→ DB = "No"
→ Reload
→ API = "No"
→ UI = "No"
→ Existing GREEN styling remains.

CASE 3:
User selects "N/A"
→ Save
→ DB = "N/A"
→ Reload
→ API = "N/A"
→ UI = "N/A"

CASE 4:
Existing records containing "Yes" or "No"
→ Must continue displaying exactly as before.

CASE 5:
Existing records containing NULL/empty values
→ Do not introduce unrelated behavior changes.

VALIDATION REQUIRED:
After making the change, verify the complete flow:

1. Select N/A for accuratelyDefinedTracked.
2. Save.
3. Inspect Network → save request.
4. Confirm payload contains:
   accuratelyDefinedTracked: "N/A"
5. Reload page.
6. Inspect GET review API response.
7. Confirm:
   accuratelyDefinedTracked: "N/A"
8. Repeat for:
   accuratelyCalculated
   breachesMitigated
9. Verify Yes still returns Yes.
10. Verify No still returns No.
11. Verify existing Yes/No colors remain unchanged.

DO NOT make broad refactors.

DO NOT change the database schema.

DO NOT change the save API contract.

DO NOT change SUS Required parsing.

DO NOT change unrelated Covenants functionality.

This should be a minimal, targeted fix to the GET-side Yn()/normalization logic that currently converts unknown values such as "N/A" to "No".

Before editing, inspect the exact usages of Yn() in GetCovenantsSectionAsync and confirm which three fields use it. Then make the smallest possible change.

After editing, provide:
1. Exact file changed.
2. Exact method changed.
3. Before → after summary.
4. Confirmation that Save logic was not changed.
5. Confirmation that Yes/No behavior and styling were preserved.
6. Confirmation that N/A now survives Save → DB → GET → UI reload.
