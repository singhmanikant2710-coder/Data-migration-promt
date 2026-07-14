Frontend only. File: frontend/src/components/ui/SearchableSelect.tsx and frontend/src/app/review-status/page.tsx.
Do not plan. Just apply.

The Select Sample Name control now renders a BOX INSIDE A BOX — there are two nested bordered elements. There must be exactly ONE bordered box.

Find every element in the render path of that control that currently has a border class:
  - the wrapper <div> around SearchableSelect in review-status/page.tsx
  - the SearchableSelect outer container div
  - the SearchableSelect trigger button (the one I asked you to give `boxed` styling)

Keep the border/rounded/bg-white/px-2/py-1/text-sm ONLY on the trigger button (the boxed branch).
Remove border, rounded, bg-white, ring, and shadow classes from EVERY other element in that chain. Those wrappers must be completely transparent and unstyled — no border, no background, no padding that creates a visible box.

Result: exactly one visible rectangle, identical in height, border, corner radius, background and font-size to the Sample Start Date input beside it.
