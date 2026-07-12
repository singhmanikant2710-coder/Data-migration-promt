Approved — Option B (Lock).

Implement:
1. When the user changes CRM Component to "06-Servicing Systems" or "07-Data Integrity", default that row's Severity to "Observation" (as per your sketch — updateRow + the changes.setSection snapshot both consistent).
2. LOCK the Severity control for those two components: disable the Select and show only "Observation" (no "Finding" option available). Re-enable the full dropdown when the component changes to anything else.
3. Do NOT overwrite severity on initial load — this applies only when the user changes the component. HOWEVER: for existing rows already saved with component 06/07, the Severity control must still render LOCKED (read-only, showing whatever is saved) so users cannot change it to "Finding" going forward. Do not silently rewrite the saved value.
4. The disabled Select must still display its current value clearly (not appear blank/greyed-out to the point of being unreadable) and must be visually consistent with other disabled controls in the app.

Single-file edit to CrmFindingsAndRatingsSection.tsx. Show me the diff. STOP if another file needs changing.
