Approved — implement UAT #57 exactly as planned. Keep FormChangesContext as-is (the mergeValues array fix stays; it is confirmed safe for all other sections since only crmFindingsAndRatings stages an array).

Implement:

1. CrmFindingsAndRatingsSection.tsx
   a. Add Row: build nextArr from (pendingFindings ?? s.findings), append the new row, stage via setSection("crmFindingsAndRatings", { findings: nextArr }), and setActiveRowId(newId).
   b. Delete Row: for an unsaved row, filter from (pendingFindings ?? s.findings), stage it, then deleteRow(row.id). For a saved row, after deleteCrmFinding succeeds, stage the filtered array then deleteRow(row.id). On failure, preserve the row exactly as today.
   c. Derive: const pendingFindings = changes?.changes?.crmFindingsAndRatings?.findings;
             const effectiveFindings = Array.isArray(pendingFindings) ? pendingFindings : (s?.findings ?? []);
      Use effectiveFindings in exactly three places: active-row init useEffect, activeDesc useMemo, and the table row render.
   d. Keep per-field edit staging as-is.

2. CrmRatingsSection.tsx — compute the same effectiveFindings and derive DISPLAY-ONLY counts (severity === "Finding", components 01–05). Wire the five StatCards with ?? 0 fallback.

Hard constraints:
- Never use { ...array } or Object.assign({}, array). Always pass real Arrays.
- Do NOT touch the save path, setRating, or the UNSAT checkboxes.
- No new API calls.
- Counts are display-only: never written to state, never included in any save payload.

Apply and show me the diff for each file. STOP after applying.
