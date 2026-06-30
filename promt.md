Reduce the gap a bit more. In page.tsx, change the content container's marginTop from Math.max(0, toolbarH - 8) to Math.max(0, toolbarH - 16).

RULES: layout-only, page.tsx only, no business logic / API / TopChromeBar changes, no packages. Show me the diff. Header must still NOT overlap content on scroll.
