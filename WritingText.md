UAT #248 – Review Form: rename comment box "Cancel" to "Undo"; confirm dialog for the top-banner Cancel

REQUIREMENT (Geoff, Issue 248)
1. The "Cancel" button in the comment (rich-text) box toolbars should read "Undo", in the comment boxes of every Review Form tab.
2. Clicking "Cancel" in the top Review Form banner (next to Edit/Save) should open a modal asking the user to confirm cancelling all edits made since the last save.
Purpose: make the difference clear between cancelling all unsaved edits (top banner) and undoing one comment box's edits, and prevent data loss.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Rich-text editor component with file:line: where the toolbar "Cancel" button is defined, and EXACTLY what it does (reverts the box to its last saved value? to the value when editing started? cancels a link/table/image dialog? clears the whole box?). Does it touch staged changes (FormChangesContext) for that field?
- Every place the editor/toolbar is used (all Review Form tabs: Customer Background, Key Risks, Covenants narrative, Policy Exceptions comments, CRM Findings, Risk Rating Justification, Checklist, etc. AND outside the Review Form: Email modal body, Help Tips/admin screens, any other). List each with file:line.
- Top banner Cancel with file:line: what it does today (discard staged changes, exit edit mode, reload, clear the #177 localStorage draft?), and every other caller of the same handler (Save-and-leave, #232 conflict refresh, #239/#240 hook, unsaved-changes navigation guard).
- Existing dialog/modal components to reuse (ConfirmDialog in Dialog.tsx, the #177 restore prompt).

PHASE 2 – PLAN (smallest generic change)
- Label: change "Cancel" to "Undo" only where the button really reverts that box's text. If the button does something else (e.g. closes a link/table dialog), report it and do not rename that one.
- Scope of the rename: Review Form tabs only, as requested. Do not change the label in the Email modal or other screens unless I approve (list them as a decision).
- Behaviour of the editor button must stay exactly the same; only the text (and tooltip/aria-label if any) changes.
- Top banner Cancel: reuse the existing ConfirmDialog. Title/message e.g. "Discard unsaved changes? All edits made since your last save will be lost." Buttons: "Discard changes" and "Keep editing". Keep editing = nothing changes; Discard = exactly today's Cancel behaviour (unchanged logic).
- Do NOT decide silently – list options with a recommendation:
  1. When there are NO unsaved changes, show the dialog anyway or cancel straight away?
  2. Should the #177 local draft also be cleared on Discard (whatever happens today must stay unless you recommend otherwise)?
  3. Email modal / other screens using the same editor: keep "Cancel" or also "Undo"?
- Must stay unchanged: Save, Edit, #177 autosave/restore, #232 conflict dialog, #239/#240 prompts, navigation guard, locked-review behaviour.
- No backend, schema or SQL change expected.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- What the editor button does (file:line); ADDED (file:line); REMOVED/CHANGED (file:line).
- On-screen before/after: comment box button label in each Review Form tab; clicking Undo (same behaviour as before); top Cancel with unsaved changes → Keep editing / Discard; top Cancel with no changes; Esc / X on the dialog.
- Edge cases: unsaved changes in several tabs; edits only in a comment box; locked review (Cancel availability unchanged); Cancel during a save in progress.
- Confirm other screens using the editor are unchanged (unless approved).
- npx tsc --noEmit, npm run build (dotnet build only if backend touched).
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.
