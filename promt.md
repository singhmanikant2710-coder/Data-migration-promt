Bug 196 EDIT fix. SINGLE FILE, ONE change only. Do NOT touch anything else. Show diff before applying.

FILE: frontend/src/services/selectionsService.ts
FUNCTION: updateSelection ONLY.

The backend PUT /api/v1/selections/library/{id} requires 'section' as a QUERY STRING param (it is [FromQuery] and implicitly required under Nullable enable + [ApiController]). Currently updateSelection sends section only in the body, so the query is empty → HTTP 400 "One or more validation errors occurred". The delete path already does this correctly.

Change updateSelection to append section as a query string, mirroring deleteSelection (lines 49-52). Extract section from the dto, URL-encode it with the same enc() helper deleteSelection uses, append ?section=<enc(section)> to the URL. Keep sending the body as-is (selection, etc.).

Example target shape (match existing code style/helpers exactly):
  export async function updateSelection(id: number, dto: UpdateSelectionDto): Promise<void> {
    const qs = `?section=${enc(dto.section)}`;
    await put<UpdateSelectionDto, unknown>(`/api/v1/selections/library/${id}${qs}`, dto);
  }

Do NOT change the controller, the DTO, the repository, the page.tsx handler, or the Add flow. Only this one function. 
Commit: "Fix Bug 196 edit: send section as query param in updateSelection (mirror delete path)".
