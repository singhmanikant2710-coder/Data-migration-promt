Frontend only. Two files. Do NOT modify any other file. Do NOT change or revert any existing UAT #55 logic (Observation default+lock for 06-Servicing Systems / 07-Data Integrity) — only ADD the CRM-00 case alongside it. Do not plan. Just apply.

UAT #108: On CRM Findings, when Finding Code = "CRM-00", the Severity must default to "N/A" and lock, exactly mirroring how #55 defaults+locks to "Observation" for components 06/07.

FILE 1: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCrmFindings.ts
- Change the Severity type from:
    export type Severity = "Observation" | "Finding";
  to:
    export type Severity = "Observation" | "Finding" | "N/A";
- Keep SEVERITY_OPTIONS unchanged (do NOT add "N/A" to it — it must not be selectable for normal rows):
    export const SEVERITY_OPTIONS: Severity[] = ["Observation", "Finding"];
- If a normalizeSeverity helper exists, add: if (v === "n/a" || v === "na") return "N/A"; Do not change its other branches.

FILE 2: frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

1) Extend severityLocked to also lock when the Finding Code is "CRM-00" (keep the existing two conditions exactly):
     const severityLocked =
       row.component === "06-Servicing Systems" ||
       row.component === "07-Data Integrity" ||
       row.findingCode === "CRM-00";

2) In the Severity <Select> options branch, replace the hardcoded locked default "Observation" with a computed default so CRM-00 shows "N/A" and 06/07 still show "Observation". Change:
     (severityLocked
       ? (row.severity ? [row.severity] : ["Observation"])
       : (severityOpts ?? SEVERITY_OPTIONS)
     )
   to:
     (severityLocked
       ? (row.severity ? [row.severity] : [row.findingCode === "CRM-00" ? "N/A" : "Observation"])
       : (severityOpts ?? SEVERITY_OPTIONS)
     )

3) In the Finding Code SearchableSelect onChange, add the default-to-"N/A" behaviour when val === "CRM-00", mirroring how the Component onChange defaults to "Observation" for 06/07. Set severity to "N/A" in the updatedFindings array, in updateRow, and in the changes.setSection snapshot. When val is not "CRM-00", leave severity as row.severity (do not clear it — matching #55, which preserves severity when switching away).

Do not touch the Component onChange (#55 logic), the disabled binding, or any other field.

Run read-only TypeScript diagnostics on both files only.
