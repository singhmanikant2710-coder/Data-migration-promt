TEMPORARY DEBUG. Add console.logs in frontend/src/app/customer/edit/page.tsx to trace the covenant threshold flow. Remove later.

1) In onCovenantRowEdit, right after computing 'next' (before the cleanup check), add:
   console.log("[COV-EDIT] index=", index, "patch=", patch, "resolved row=", covenants[index]?.name, "key=", key, "next=", next);

2) In onSaveCovenantFields, right after `const entries = Object.entries(covEdits);`, add:
   console.log("[COV-SAVE] covEdits entries=", JSON.stringify(entries));

3) In onSaveCovenantFields, right before the by-name/id call (after effectivePayload is finalized), add:
   console.log("[COV-PAYLOAD] key=", key, "effectivePayload=", JSON.stringify(effectivePayload));

Apply these three logs only.
