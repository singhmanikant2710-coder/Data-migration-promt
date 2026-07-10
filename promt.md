Single-file edit to useCrmFindings.ts only. Show diff before applying.

Two fixes to the label-building effect:

FIX 1 (SS and other components not getting labels): The `if (labelMap[comp]) continue;` guard can permanently skip a component whose entry was set before its library resolved. Change the guard so it only skips when the component ALREADY has a non-empty label map with at least one "code - description" entry. Replace:
  if (labelMap[comp]) continue;
with:
  const existing = labelMap[comp];
  const hasLabels = existing && Object.values(existing).some(v => v.includes(" - "));
  if (hasLabels) continue;

FIX 2 (consistent formatting): build each label as a single clean format:
  map[code] = desc ? `${code} - ${desc}` : code;
(single " - " separator, not em-spaces).

Keep the in-flight guard, deps [codeMap, labelMap], build directly from library items (findingCode/description), catch fallback, and option value = code. Do NOT touch save logic.

Show diff. STOP if another file needs changing.
