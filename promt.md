The vertical gap between the TopChromeBar header and the content below it is a bit too large now. Reduce it slightly.

In page.tsx, the content container currently uses style={{ marginTop: toolbarH }}. Reduce this by a small fixed amount so the gap tightens but content still doesn't overlap the header on scroll. Use something like style={{ marginTop: Math.max(0, toolbarH - 8) }} (try -8px first; if still too much we can go to -12 or -16).

RULES: layout-only, page.tsx only, no business logic / API / TopChromeBar changes, no packages. Show me the diff. The header must still NOT overlap content when scrolling — only the resting gap should shrink.
