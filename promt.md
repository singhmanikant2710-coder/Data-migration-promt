FIX REQUEST — Policy Exceptions "Identified" N/A VALUE IS LOST AFTER SAVE

Issue:
Policy Exceptions → Identified field supports:
- Yes
- No
- N/A

When the user selects "N/A" and clicks Save, the UI/request can contain "N/A", but the backend UpsertPolicyExceptionsAsync normalization converts every string value other than Yes/Y/True/1 into "No".

Current problematic logic is inside:

backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Method:

public async Task<RowResult[]> UpsertPolicyExceptionsAsync(
    int reviewId,
    TrackedItem<System.Text.Json.JsonElement>[] items,
    CancellationToken ct)

Current string handling is effectively:

case System.Text.Json.JsonValueKind.String:
    var s = (vId.GetString() ?? "").Trim().ToLowerInvariant();
    identified = (s is "yes" or "y" or "true" or "1") ? "Yes" : "No";
    break;

This causes:
"N/A" → "No"

REQUIRED FIX:

Update ONLY the normalization of the "identified" value in UpsertPolicyExceptionsAsync so it supports all three states:

- Yes / Y / True / 1 → "Yes"
- No / N / False / 0 → "No"
- N/A → "N/A"
- n/a / N/a / NA (if the existing UI/request can send these forms) should also normalize to "N/A"
- null/empty/unknown values should preserve the existing behavior unless the current code has an explicit established rule for them.

Preferred replacement for the STRING branch:

case System.Text.Json.JsonValueKind.String:
    var s = (vId.GetString() ?? "").Trim();

    if (s.Equals("yes", StringComparison.OrdinalIgnoreCase) ||
        s.Equals("y", StringComparison.OrdinalIgnoreCase) ||
        s.Equals("true", StringComparison.OrdinalIgnoreCase) ||
        s == "1")
    {
        identified = "Yes";
    }
    else if (s.Equals("no", StringComparison.OrdinalIgnoreCase) ||
             s.Equals("n", StringComparison.OrdinalIgnoreCase) ||
             s.Equals("false", StringComparison.OrdinalIgnoreCase) ||
             s == "0")
    {
        identified = "No";
    }
    else if (s.Equals("n/a", StringComparison.OrdinalIgnoreCase) ||
             s.Equals("na", StringComparison.OrdinalIgnoreCase))
    {
        identified = "N/A";
    }
    else
    {
        // Preserve the existing fallback behavior.
        identified = "No";
    }
    break;

IMPORTANT:
Do NOT make broad changes to Policy Exceptions.
Do NOT change the GET/read method.
Do NOT change the controller.
Do NOT change ReviewService.
Do NOT change the database schema.
Do NOT change SavePolicyExceptionsInfoAsync.
Do NOT change unrelated fields such as description, category, exception code, mitigation, notes, etc.
Do NOT change the Covenants implementation or its N/A normalization.
Do NOT change existing Yes/No behavior.
Do NOT refactor the whole UpsertPolicyExceptionsAsync method.

Only make the minimal targeted change required so Policy Exceptions → Identified preserves N/A.

Existing behavior that MUST remain unchanged:

1. Identified = true JSON boolean → "Yes"
2. Identified = false JSON boolean → "No"
3. Identified = numeric 1 → "Yes"
4. Identified = numeric 0 → "No"
5. Identified = "Yes" → "Yes"
6. Identified = "No" → "No"
7. Identified = "N/A" → "N/A"

The existing SQL upsert already writes the normalized value through:

[Exception_identified] = @identified

and:

INSERT INTO dbo.[02_CORE_06_Policy_Exceptions]
    ([Exception_code], [Review_id], [Exception_identified], [Exception_description], [Exception_category])
VALUES
    (@code, @id, @identified, @newDesc, @category);

Do not modify this SQL unless compilation or an actual type issue requires it.

Validation required after the change:

1. Open Review Form → Policy Exceptions.
2. For "E00 - No Policy Exceptions", select Identified = "N/A".
3. Save.
4. Verify the save request contains "N/A".
5. Verify database column [Exception_identified] contains "N/A".
6. Reload the Review Form.
7. Verify GET returns "N/A".
8. Verify UI displays "N/A".
9. Repeat with "Yes" and confirm it still displays "Yes".
10. Repeat with "No" and confirm it still displays "No".
11. Confirm no unrelated Policy Exception fields changed.
12. Confirm Covenants N/A behavior remains untouched.

Before editing:
- Inspect the complete UpsertPolicyExceptionsAsync method.
- Make the smallest possible change.
- Do not overwrite or restructure the entire method.
- Preserve existing transaction handling, error handling, RowResult generation, SQL, and all other mappings.

After editing:
- Show exactly which lines were changed.
- Confirm only backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs was modified.
- Build the affected backend project and report any compilation errors.
