Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

Bug #179: CRM Findings delete uses a browser-native window.confirm("Delete this 
finding?") popup instead of the app's ConfirmDialog. Replace the native confirm 
with the shared ConfirmDialog, matching the working PolicyExceptionsSection 
pattern. Do NOT remove confirmation entirely (delete must still be confirmed) — 
swap native confirm for the app modal.

Reference (working) pattern from PolicyExceptionsSection.tsx:
- useState for confirmOpen + pendingDeleteId
- openDeleteModal(id) sets pendingDeleteId + confirmOpen=true
- Delete button onClick calls openDeleteModal(row.id) (no window.confirm)
- <ConfirmDialog open={confirmOpen} ... onConfirm={handleDeleteConfirm} .../>
- handleDeleteConfirm does the actual delete (API + UI update)

Apply the same to CRM Findings:

STEP 1 — Import ConfirmDialog (from frontend/src/components/ui/Dialog.tsx) if 
not already imported.

STEP 2 — Add state (near other useState in this component):
    const [confirmOpen, setConfirmOpen] = useState<boolean>(false);
    const [pendingDeleteRow, setPendingDeleteRow] = useState<any | null>(null);

STEP 3 — In the Delete button onClick, REMOVE the window.confirm and the 
inline delete logic for SAVED rows. Keep the NEW-unsaved-row local delete as-is 
(that path has no findingCode and deletes locally — it doesn't need a modal). 
For SAVED rows (has findingCode), instead of window.confirm + delete, open the 
modal:

Current onClick (saved-row portion):
    const confirmed = typeof window !== "undefined" ? window.confirm("Delete this finding?") : true;
    if (!confirmed) return;
    try {
      if (!Number.isFinite(reviewId) || reviewId <= 0) return;
      await deleteCrmFinding(reviewId, code);
      ... UI update ...
    } catch (err) { ... }

Change the Delete button so, for a saved row, it opens the modal instead:
    onClick={async () => {
      const code = (row.findingCode ?? "").trim();
      if (!code) {
        // NEW unsaved row: keep existing local delete (unchanged)
        const base = Array.isArray(pendingFindings) ? pendingFindings : (s?.findings ?? []);
        const nextArr = base.filter((f) => f.id !== row.id);
        if (changes) changes.setSection("crmFindingsAndRatings", { findings: nextArr });
        deleteRow(row.id);
        return;
      }
      // SAVED row: open the app confirm modal (no window.confirm)
      setPendingDeleteRow(row);
      setConfirmOpen(true);
    }}

STEP 4 — Add a handleDeleteConfirm function that runs the actual saved-row 
delete (moved from the old onClick try/catch), using pendingDeleteRow:
    async function handleDeleteConfirm() {
      const row = pendingDeleteRow;
      setConfirmOpen(false);
      if (!row) return;
      const code = (row.findingCode ?? "").trim();
      try {
        if (!Number.isFinite(reviewId) || reviewId <= 0) return;
        await deleteCrmFinding(reviewId, code);
        // ... the exact same UI-update / pending-array / sessionStorage / 
        //     router.replace logic that was previously after the await ...
      } catch (err) {
        // keep row on error (same as before)
      } finally {
        setPendingDeleteRow(null);
      }
    }

STEP 5 — Render the ConfirmDialog once in the component's JSX (like Policy 
Exceptions), with a message referencing a FINDING (not policy exception):
    <ConfirmDialog
      open={confirmOpen}
      onClose={() => { setConfirmOpen(false); setPendingDeleteRow(null); }}
      onConfirm={handleDeleteConfirm}
      danger
      title="Confirm Delete"
      message="Are you sure you want to delete this finding? This action cannot be undone."
      confirmText="Delete"
      cancelText="Cancel"
    />

CONSTRAINTS:
- Remove the window.confirm entirely; use ConfirmDialog instead (no native popup).
- Keep the NEW-unsaved-row local delete path unchanged (no modal for those).
- Move the existing saved-row delete logic (deleteCrmFinding + UI update + 
  sessionStorage clear + router.replace) into handleDeleteConfirm UNCHANGED — 
  do not alter the delete API call or the post-delete refresh logic.
- Message says "finding" (not "policy exception").
- Only edit this one file. Do NOT modify Dialog.tsx or PolicyExceptionsSection.
- Show: the new state, the updated Delete onClick, handleDeleteConfirm, and the 
  ConfirmDialog JSX.
