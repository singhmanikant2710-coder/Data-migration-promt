Bug 198 (part 2) — Delete modal consistency on Samples/Load Samples screen. READ-ONLY, no edits. One pass, answer, STOP.

The Samples (Load Samples) screen deletes a customer using the browser's native window.confirm() (shows "webapp says... Delete customer? OK/Cancel"). Other screens (e.g. CRM Findings, Review Form) use a styled in-app "Confirm Delete" modal ("Are you sure you want to delete this? Cancel/Delete"). Goal: make the Samples delete use the SAME styled modal, removing the native confirm().

Report:
1. In the Load Samples page (frontend/src/app/load-samples/page.tsx or its delete handler), find the customer-delete logic. Does it call window.confirm() / confirm()? Paste that exact handler + the line that triggers the native popup.
2. Find the reusable styled confirm-delete modal component used by other screens (search for "Confirm Delete", "cannot be undone", ConfirmDialog, DeleteModal, ConfirmModal, Dialog). Report the component name + file path + its props (e.g. open, title, message, onConfirm, onCancel).
3. Show one example of how another screen (e.g. CRM Findings) uses that modal — the state (useState open) + JSX usage, so I can mirror the pattern.
4. Confirm whether the Load Samples page already imports/uses that modal anywhere (so we reuse, not recreate).

Report file paths + the reusable modal's API. Do NOT change anything.
