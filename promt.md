Backend only. Single file: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs
Method: GetCollateralTrackingSectionAsync
Use LIVE DB, ignore columns.csv. Do NOT modify or revert any other logic (including Jothi's). Do not plan. Just apply.

UAT #167: On the Collateral Summary table, the FACILITY NUMBER column is empty because the query selects only [Facility_number_calc], which is NULL for these rows. It should fall back to [Facility_number] when [Facility_number_calc] is NULL.

In the const string sql of GetCollateralTrackingSectionAsync, make exactly these three changes so the calc value is preferred and Facility_number is the fallback:

1) SELECT — change:
     a.[Facility_number_calc] AS FacilityNumber,
   to:
     COALESCE(a.[Facility_number_calc], a.[Facility_number]) AS FacilityNumber,

2) GROUP BY — change:
     GROUP BY a.[Facility_number_calc]
   to:
     GROUP BY COALESCE(a.[Facility_number_calc], a.[Facility_number])

3) ORDER BY — change:
     ORDER BY a.[Facility_number_calc]
   to:
     ORDER BY COALESCE(a.[Facility_number_calc], a.[Facility_number])

Do NOT change any other column, the WHERE clause, the mapping, or anything else. No frontend changes.



Read-only. No edits. No plan. Just report file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #168): Add "Font Color" and "Font Highlight" formatting buttons to all rich text fields in the Review Form. All rich text fields use the shared RichTextEditor component, so one change should apply everywhere.

Report:
1) In frontend/src/components/ui/RichTextEditor.tsx: paste the toolbar section where the existing formatting buttons (B, I, U) are rendered, and paste the execCmd helper (the one that calls document.execCommand). Show how B/I/U are wired.
2) Does execCommand support 'foreColor' (font color) and 'hiliteColor' (highlight) here? Are there any existing color-related buttons? 
3) How would a color value be chosen — is there any existing color picker component in the codebase, or would a simple <input type="color"> be needed? Note what's available.
4) Confirm these buttons would automatically appear in every rich text field (since RichTextEditor is shared), EXCEPT where hideButtons is used (recall UAT #109 hid some buttons only in CRM Findings). Should Font Color / Highlight also be hideable via that same prop, or always shown?
5) State exactly what must change and in how many files to add Font Color and Font Highlight buttons to the shared editor.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.
