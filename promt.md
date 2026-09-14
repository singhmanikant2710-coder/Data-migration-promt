Bug 207 follow-up fix. Apply all 4 changes. Show diffs, rebuild, run node --test, do NOT commit.

FILE: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCovenants.ts

1. Line ~52 — FREQUENCY_PRESET: add "N/A" to the array (e.g. append after "Other", or in a sensible position).

2. Line ~416 — eval status base list: add "N/A" to the array.

3. Line ~75 (normEvalStatus) — the regex currently matches \bn/a\b and maps it to "Not Due". Change this so "N/A" is preserved as its own value, NOT collapsed into "Not Due". Keep all other normalization behavior (Compliant/Not-Compliant/Waived/Past Due mappings) unchanged — only carve out N/A into its own passthrough branch.

FILE: frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx

4. Lines ~322-350 (the existing Covenant Type onChange, which already resolves getLibraryMetaForType(val) and writes sourceCategory/covenantTypeCode/covenantCategoryOrder into both updateRow(...) and stagePatch(...)): 
   When val.toLowerCase() is "no monitoring covenants" OR "no performance covenants", ALSO set frequency: "N/A" and lastEvalStatus: "N/A" in that SAME updateRow(...) and stagePatch(...) call (do not add a separate useEffect/ref — use this existing onChange hook only, per the CustomerInfo lesson from UAT #177).
   For any OTHER covenant type selected, do NOT force these fields (leave them as user-set or blank as today).

Do NOT touch the backend (both columns are unvalidated NVARCHAR(50), no DTO change needed — confirmed). Do NOT use a useEffect+ref cascade pattern.

Show diffs. Rebuild. Run node --test. Do NOT commit.
