Approved — apply the plan exactly as proposed. Single file: RiskRatingJustificationSection.tsx.

For the false case, use "No" (value={rrUnsat ? "Yes" : "No"}).

Hard constraints (repeat):
- DISPLAY-ONLY: no writes to state, nothing added to any save payload, no new API calls.
- Do NOT touch the save path, setJustification, or the UNSAT checkboxes on CRM Ratings.
- Must react live to unsaved edits (pending FormChangesContext snapshot) exactly like the #57 StatCards.
- Confirm that importing useCrmFindings() into this component does NOT trigger an extra network request (it must reuse the shared ReviewDataContext payload).

Apply and show me the diff. STOP after applying.
