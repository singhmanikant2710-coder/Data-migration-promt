The isEditing fix did not help — the pending row still disappears. So `pending` itself is empty/undefined when we return to CRM Findings. Stop reading code; prove it at runtime.

Apply a TEMPORARY debug log (single file, CrmFindingsAndRatingsSection.tsx) — I will remove it after. Inside rowsToRender, before the return:

console.log("CRM_ROWS_DEBUG", {
  hasChanges: !!changes,
  changesKeys: Object.keys((changes as any)?.changes ?? {}),
  pendingSection: (changes as any)?.changes?.crmFindingsAndRatings,
  pendingFindingsCount: Array.isArray((changes as any)?.changes?.crmFindingsAndRatings?.findings)
    ? (changes as any).changes.crmFindingsAndRatings.findings.length
    : "NOT_AN_ARRAY",
  savedFindingsCount: (s?.findings ?? []).length,
});

ALSO — in the same task, report (read-only) the implementation of `setSection` in FormChangesContext: does it MERGE the payload into the existing section object, or REPLACE the whole section object? Show the code verbatim.

Apply the log and report setSection. STOP after that.
