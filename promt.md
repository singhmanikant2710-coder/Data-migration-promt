Read-only first, no edits: In CrmFindingsAndRatingsSection.tsx, show me the EXACT current JSX of the Finding Code <option> rendering (the codeOptions.map block). I need to see whether the option text is `{code}` or `{labelOptions[code] ?? code}`, and confirm labelOptions is defined in that scope as `labelOpts[row.component] ?? {}`.

Report the exact lines. STOP before editing.

Single-file edit to CrmFindingsAndRatingsSection.tsx only.

In the Finding Code options map, change the option display text to use the label with fallback. Ensure just above the map:
  const labelOptions = row.component ? (labelOpts[row.component] ?? {}) : {};
And the option:
  <option key={code} value={code}>{labelOptions[code] ?? code}</option>

Keep value={code} unchanged. Do NOT touch save logic. Confirm labelOpts is destructured from useCrmFindings as FINDING_LABELS. Show diff. STOP if another file needs changing.
