The previous diff was wrong — it calls put() TWICE (first without query string = the broken call, then with). Fix updateSelection to make EXACTLY ONE put() call, with the section query string. Remove the first duplicate call entirely.

FILE: frontend/src/services/selectionsService.ts — updateSelection ONLY.

Final function must be exactly this shape (single call):

  export async function updateSelection(id: number, dto: UpdateSelectionDto): Promise<void> {
    const qs = `?section=${enc(dto.section ?? "")}`;
    await put<UpdateSelectionDto, unknown>(`/api/v1/selections/library/${id}${qs}`, dto);
  }

Only one put() call. No duplicate. Do not touch anything else. Show the diff.
