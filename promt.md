Bug 198 part 2 fix — replace native window.confirm on Load Samples delete with the existing ConfirmDialog modal. SINGLE FILE. Mirror the CRM Findings pattern exactly. Show diff before applying.

FILE: frontend/src/app/load-samples/page.tsx

Changes:
1. Line 14 import: add ConfirmDialog to the existing import:
   import { InfoDialog, Modal, ConfirmDialog } from "@/components/ui/Dialog";

2. Add two state hooks near the page's other useState declarations:
   const [confirmDeleteOpen, setConfirmDeleteOpen] = useState<boolean>(false);
   const [pendingDeleteRow, setPendingDeleteRow] = useState<SampleLoad | null>(null);

3. In deleteChildRow (lines 1132-1190): KEEP the guards and the staged/unsaved early-return path (lines 1134-1153) EXACTLY as-is. REMOVE the native window.confirm block (lines 1154-1158). Instead, for persisted rows, open the modal:
   setPendingDeleteRow(r);
   setConfirmDeleteOpen(true);
   return;
   Move the API portion (the deleteSampleLoad call + state update + toasts + catch/finally, lines 1160-1189) into a new async handler handleDeleteConfirm that reads the pending row into a local variable FIRST:
   async function handleDeleteConfirm() {
     const r = pendingDeleteRow;
     setConfirmDeleteOpen(false);
     if (!r) return;
     const sid = Number(selectedParentId);
     setChildSaving(true);
     try { ... existing API + state + success toast ... }
     catch { ... existing not-found/staged + error handling ... }
     finally { setChildSaving(false); setPendingDeleteRow(null); }
   }
   Preserve all existing toast messages, the not-found→staged fallback, and setChildSaving behavior exactly.

4. Render one <ConfirmDialog> near the page's other Modal/InfoDialog instances (around line 2441):
   <ConfirmDialog
     open={confirmDeleteOpen}
     onClose={() => { setConfirmDeleteOpen(false); setPendingDeleteRow(null); }}
     onConfirm={handleDeleteConfirm}
     danger
     title="Confirm Delete"
     message={`Are you sure you want to delete customer ${pendingDeleteRow?.customer_number ?? ""}? This action cannot be undone.`}
     confirmText="Delete"
     cancelText="Cancel"
   />

Do NOT change the row Delete button onClick (it still calls deleteChildRow(r)). Do NOT touch the staged/unsaved local-delete path. Do NOT change any other modal on the page. This is the only window.confirm in the frontend — after this, there should be zero.

List every line changed. Commit: "Fix Bug 198 part 2: replace native confirm with ConfirmDialog on Load Samples delete".
