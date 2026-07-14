Frontend only. File: frontend/src/components/ui/SearchableSelect.tsx AND frontend/src/app/review-status/page.tsx.
Do not plan. Do not explain. Just apply.

CONTEXT: The date inputs on Review Status use exactly:
  w-full border rounded px-2 py-1 text-sm text-slate-800 bg-white
The SearchableSelect trigger currently uses:
  bg-transparent border-0 rounded-none px-0, and h-[16px] leading-[12px] text-[11px] for the default size
That is why the dropdown looks tiny and borderless next to the date fields.

DO NOT change SearchableSelect's default styling globally — other screens depend on it.

STEP 1 — SearchableSelect.tsx
Add a new optional prop:  boxed?: boolean   (default false)
When boxed is true, the trigger className must instead be:
  "inline-flex items-center justify-between w-full font-sans border rounded px-2 py-1 text-sm text-slate-800 bg-white focus:outline-none focus-visible:ring-1 focus-visible:ring-[color:var(--brand-primary)]/40"
When boxed is false, keep the existing className exactly as it is today. Do not modify the existing branch.

STEP 2 — SearchableSelect.tsx, dropdown menu items
Add another optional prop:  boldOptions?: boolean  (default false)
When boldOptions is true, add "font-semibold text-slate-800" to the option row className (the baseRowClasses branch). When false, leave the current row className untouched.

STEP 3 — review-status/page.tsx
On the "Select Sample Name" SearchableSelect only, pass boxed and boldOptions:
  <SearchableSelect ... boxed boldOptions />
Also make the closed trigger's selected text bold (e.g. "Select All" must render bold) — if the trigger renders the selected label in a span, add font-semibold to it when boxed is true.

Do not change the dropdown's width, the grid, the counts, or any other page.

Report done when the Select Sample Name control visually matches the Sample Start Date input: same height, same border, same rounded corners, same text-sm size, white background.
