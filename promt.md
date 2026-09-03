Bug 196 — CASRR UAT (reported by Geoffrey Houlditch, client stakeholder). READ-ONLY diagnosis. NO edits. One pass, answer all, STOP.

GEOFF'S EXACT REPORTED ISSUE:
Screen/Tab: Libraries > Selections (Library Maintenance)
Section: Report Selections (Reporting tab)
Geoff's words: "I am unable to edit the Reporting name selection." He clicks Edit on an existing selection row (e.g. "PD Grade Migration", Selection ID 7), changes the Reporting name text, clicks Save, and gets the inline error "One or more validation errors occurred." The edit does not save. (Screenshot attached showing the row in edit mode with the validation error.)

CONTEXT ALREADY KNOWN:
- Table dbo.[03_LIBRARY_09_Selections] has composite PRIMARY KEY (Section, Selection_id).
- The ADD flow was already fixed separately (server-side per-section Selection_id generation) and works. DO NOT touch the Add flow.
- This remaining failure is on the EDIT/UPDATE path only, and it is an HTTP 400 model-validation error ("One or more validation errors occurred" is ASP.NET's default 400 validation response).

TRACE THE EDIT PATH ONLY. Open and report:
1. SelectionsController.cs — the PUT library/{id} method (UpdateLibraryItem): full signature, the [FromQuery] section param, the [FromBody] DTO, and any validation attributes. What exactly causes a 400 model-validation error on this endpoint?
2. UpdateSelectionLibraryItemDto — paste ALL properties and any DataAnnotations ([Required], [StringLength], [Range], etc.).
3. frontend page.tsx — the Edit Save handler + updateSelection service: paste the EXACT request it sends on edit — URL, HTTP method, query params, and body keys/values. Confirm whether 'section' (query param), 'tab', and 'selection' are actually populated and non-empty when editing.
4. Compare DTO requirements vs what the frontend sends on edit. Identify the EXACT field (or query param) that is required by the backend but missing/null/empty from the frontend edit request — that is what triggers the 400.

REPORT: the exact validation-failing field, with file paths + line numbers. Do NOT propose or write any fix yet.
