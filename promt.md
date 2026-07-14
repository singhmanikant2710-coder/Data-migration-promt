Approved. Apply all three fixes now:
1. SqlReviewRepository.GetFindingCodesLookupAsync — filter Active=1 (also apply to the findings/library query for the modal).
2. SearchableSelect — add the optional grid props (default to current behaviour when not passed, so other selects are unaffected).
3. Wire the Finding Code dropdown to use them: two-column grid with sticky header, uniform rows, outer border; renderSelected returns only the code.

Apply and show me the diffs.
