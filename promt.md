Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/CrmRatingsSection.tsx

The client design has NO "general" rationale box. It only has per-component UNSAT 
checkboxes, each with its own rationale editor that appears when checked. 

Remove the standalone "general" rationale editor (the one at the top with 
placeholder "Discuss rationale for UNSAT rating" that is NOT tied to a specific 
UNSAT checkbox). 

Specifically:
- Remove the general RichTextEditor and its label/container.
- Remove the "general" entry from the local rationales state and any 
  setSection("crmFindingsAndRatings", { rationales: { general: ... } }) call.
- Keep ALL 5 per-component UNSAT checkboxes and their per-component rationale editors 
  exactly as they are (riskRecognition, scorecardManagement, underwriting, 
  creditServicing, loanAdministration).

Modify ONLY CrmRatingsSection.tsx. Do not touch backend or other files.
