Bug 206 — Policy Exceptions: users cannot select "EXCEPTION MITIGATION" dropdown or save comments/edits to the Policy Exceptions comments field WITHOUT first loading a Policy Exception record. READ-ONLY, no edits. One pass, answer, STOP.

Screen: Review Form > Policy Exceptions section.

Investigate:
1. Find the Policy Exceptions section component in the Review Form (frontend). Locate the "EXCEPTION MITIGATION" dropdown and the Policy Exceptions comments field.
2. What condition currently gates/disables these fields? Find the code that disables or blocks the Mitigation dropdown and the comments field until a Policy Exception record is loaded/present. Paste the exact disabled/guard logic (e.g. disabled={!record} or a check on records.length, or a "must load first" guard in the save handler).
3. How is a Policy Exception record "loaded"? What creates/loads the record that currently unlocks these fields? Is there a separate load/add action the user must do first?
4. For SAVING: does the save handler for mitigation/comments require an existing record id? Paste the save logic — does it fail or no-op when no record exists?
5. What is the minimal change to allow the Mitigation dropdown and comments to be editable + saveable WITHOUT first requiring a loaded Policy Exception record? (e.g. remove the disabled guard, and make save create-or-update rather than update-only.) Report the exact gating sites (file + line) that need changing.

Report file paths + line numbers + the exact gating/disable logic. Do NOT fix yet.
