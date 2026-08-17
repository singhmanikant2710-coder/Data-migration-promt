READ-ONLY. Diagnostics only. Do not change anything.

Bug #179: Deleting a CRM Finding shows TWO confirmations — a browser-native 
popup ("Delete this finding?" OK/Cancel) AND the app's "Confirm Delete" modal. 
Only the app modal should show. Note: the app modal text says "delete this 
policy exception" (not "finding") — suggesting a shared/copied component.

Investigate the CRM Findings delete flow (no edits). Start with these likely 
files, then follow the flow:
- The CRM Findings table component (search: "CRM Findings" or "findings" in 
  frontend/src/app/review — likely a section component like 
  CrmFindingsSection.tsx or similar)
- The shared confirm-delete modal component

Show:

1. CRM FINDINGS TABLE: The component rendering the CRM Findings table and its 
   Delete action/button. Show the Delete button's onClick handler.

2. NATIVE CONFIRM: Search the CRM Findings delete handler (and any util it 
   calls) for a browser-native confirm:
   - window.confirm, confirm(, globalThis.confirm
   Show if the delete handler calls window.confirm("Delete this finding?") 
   BEFORE (or in addition to) opening the app modal. Show the exact line and 
   file.

3. APP MODAL: Where the "Confirm Delete" app modal is opened for CRM Findings — 
   the modal state (e.g. setConfirmOpen(true)) and the modal component. Show 
   how the delete handler opens it.

4. DOUBLE TRIGGER: Determine if the SAME handler does BOTH — calls 
   window.confirm AND opens the app modal (that's the bug), or if two layers 
   each add a confirm. Show the handler's full body so I can see both.

5. WORKING REFERENCE: Show a screen where the app confirm modal works correctly 
   WITHOUT a native popup (e.g. Policy Exceptions, since the modal text mentions 
   "policy exception" — likely CRM Findings copied that modal but ALSO kept/added 
   a window.confirm). Show that working delete handler for comparison — does it 
   open the app modal WITHOUT any window.confirm?

6. SHARED vs SPECIFIC: Is the confirm modal shared (same component used by 
   Policy Exceptions and CRM Findings)? Is the window.confirm specific to CRM 
   Findings' handler? Confirm whether removing the window.confirm from the CRM 
   Findings handler leaves the app modal intact.

7. The modal message text: it says "policy exception" even for findings — show 
   where that text comes from (is the message hardcoded/shared, or does CRM 
   Findings pass a wrong message?). (Secondary — the main bug is the double 
   confirm, but note this text mismatch.)

Do NOT edit. Show: the CRM Findings delete handler (full body — both 
window.confirm and app-modal-open), the working reference handler (app modal 
only, no native confirm), whether the modal is shared, and the exact 
window.confirm line to remove. Findings only.
