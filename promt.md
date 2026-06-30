Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

PROBLEM: Each field-change handler stages a PARTIAL, row-id-keyed object into 
FormChangesContext, e.g.:
    changes.setSection("crmFindingsAndRatings", { findings: { [row.id]: { followUp: val } } });
This makes the save payload send "findings" as an object with only changed fields. 
The backend expects "findings" as a COMPLETE ARRAY of full finding objects.

FIX: In EVERY field-change handler (component, findingCode, info, severity, 
comments, followUp), change the setSection call so it stages the FULL findings 
array with all fields, computed inline from the current s.findings (do NOT rely 
on hook state after updateRow, since that is stale).

For each handler, build the updated array inline like this pattern, then stage it:

    const updatedFindings = s.findings.map((r) =>
        r.id === row.id ? { ...r, <changedKey>: <newValue> } : r
    );
    updateRow(row.id, "<changedKey>", <newValue>);   // keep existing UI update
    if (changes) {
        changes.setSection("crmFindingsAndRatings", {
            findings: updatedFindings.map((f) => ({
                id: f.id,
                component: f.component,
                findingCode: f.findingCode,
                info: f.info ?? null,
                severity: f.severity,
                comments: f.comments,
                followUp: f.followUp,
            })),
        });
    }

Apply this to ALL field-change handlers in this file. The key change is: 
"findings" must be an ARRAY of complete finding objects (every row, every field), 
NOT a row-id-keyed partial object.

Do NOT change useCrmFindings.ts, FormChangesContext.tsx, or page.tsx. 
Modify ONLY CrmFindingsAndRatingsSection.tsx. If a change is needed in another 
file, STOP and tell me first.
