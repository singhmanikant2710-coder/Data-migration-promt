Bug 196 EDIT path still fails with "One or more validation errors occurred" (HTTP 400). READ-ONLY, no edits. One pass, answer, STOP.

1. SelectionsController.cs UpdateLibraryItem (PUT library/{id}) — paste full signature + validation attributes on UpdateSelectionLibraryItemDto and on the 'section' query param.
2. UpdateSelectionLibraryItemDto — paste all properties + any [Required]/DataAnnotations.
3. frontend page.tsx edit Save handler + updateSelection service — paste the exact URL, method, query params, and body keys sent on EDIT.
4. Which required DTO field or query param does the frontend NOT send on edit? Name the exact field causing the 400.

Do NOT fix. Just report the mismatched field.
