Single-file edit: frontend/src/components/AppShell.tsx

Fix: The <main> content wrapper uses overflow-x-hidden, which CLIPS page content wider than the viewport (cutting off the right side — Search button, pagination) instead of allowing horizontal scroll. Change it to overflow-x-auto so wide page content scrolls instead of being clipped.

BEFORE:
<main className="flex-1 min-w-0 min-h-0 overflow-x-hidden overflow-y-auto scrollbar-default scrollbar-gutter-stable p-4 md:p-6 pb-16">

AFTER:
<main className="flex-1 min-w-0 min-h-0 overflow-x-auto overflow-y-auto scrollbar-default scrollbar-gutter-stable p-4 md:p-6 pb-16">

CONSTRAINTS (UAT is running — this is a SHARED layout affecting ALL pages):
- ONLY change overflow-x-hidden -> overflow-x-auto on this one <main> element.
- Do NOT change overflow-y-auto, the flex classes, min-w-0, min-h-0, padding, or scrollbar classes.
- Do NOT change the header, footer, sidebar, or the inner content div.
- Do NOT touch any other file.
- Show the diff.
