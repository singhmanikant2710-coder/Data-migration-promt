READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx

For #176 (Samples screen refinements). Show me ONLY (no edits):

=== ITEM 1 — "Select..." default for EIC Name, Type, Target (BU) ===
1. The three dropdowns in the Select Sample grid: EIC Name, Type, Target (BU). Show each one's JSX — the component (SearchableSelect / native select?), its value, options, and placeholder. Specifically: does the "Type" dropdown have a placeholder / default "Select..." label? Show why Type currently shows no default (per Geoff, the last edit removed its default label).

=== ITEM 2 & 3 — Quarter + Closed columns conditional visibility ===
2. The Select Sample grid column definitions — show the columns array (Sample Name, Start, End, Quarter, EIC Name, Type, Target BU, Closed, etc.). Show how columns are defined.
3. Is there any existing flag/state that distinguishes "creating a new sample" vs "searching/viewing existing samples"? (e.g. an isNew row, editing mode, a "new" row marker). Show it — I need to know how to conditionally hide Quarter/Closed only in create mode.
4. The Quarter and Closed column definitions specifically — show them so I can conditionally include/exclude them.

=== Width (item 2) ===
5. The width styling on EIC Name, Type, Target (BU) dropdown cells/columns — current widths (w-44 etc.). Show what controls their width.

Read once. Findings only. No edits. Flag if the grid/columns are shared with other views.
