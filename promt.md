UAT #53 follow-up — UX polish for the Finding Code dropdown.

The dropdown now correctly shows "CODE - Description", but the native <select> is unusable for the client:
- Long descriptions make the dropdown extremely WIDE (horizontal scrolling across the screen)
- Text is truncated, no wrapping
- The closed select box shows a cut-off label like "CS-104 - Covenar..."

Requirement — replace the native <select> for the Finding Code field ONLY (in CrmFindingsAndRatingsSection.tsx) with a custom dropdown that:
- FIXED width matching the column (never stretches the page horizontally)
- Each option renders as two parts: the CODE (left, fixed narrow width) and the DESCRIPTION (right, wraps to multiple lines if long) — like the client's Access two-column layout
- VERTICAL scroll with a max-height (~300px) for long lists
- A type-to-search/filter input at the top to quickly find a code or description text
- CLOSED state shows only the selected CODE (e.g. "CS-104"), not the long label
- Selected VALUE stays the raw finding code — save/persist path completely unchanged
- Keyboard accessible: arrow keys, Enter to select, Esc to close, closes on outside click
- Matches the existing app styling (dark navy headers #1F3864, same font/spacing as other dropdowns)

FIRST: search the codebase (frontend/src/app/review/[ecif]/review-info/components/ui.tsx and any shared components/ folders) for an existing reusable combobox/searchable-dropdown component and REUSE it if one exists — do not build new if we already have one.

Report what you find and your plan (which files it touches) BEFORE editing. Do NOT touch the hook or save logic. STOP and wait for approval.
