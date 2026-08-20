Edit ONLY frontend/src/components/pdf/HtmlRichText.tsx. Auto-approve OFF.
Output diffs, no auto-apply. Do NOT touch entity decoding, parsing, or any other
component. These changes are scoped to the table styles + renderTableAst and the
image styles + renderImageAst only.

Add ONE small local helper (place it near the other helpers in this file) to
parse a CSS/attr length into a value+unit, reused by both table and image code:

  function parseLen(raw?: string | number | null): { value: number; unit: "px" | "%" } | null {
    if (raw == null) return null;
    const s = String(raw).trim();
    if (!s) return null;
    const m = s.match(/^(-?\d*\.?\d+)\s*(px|%)?$/i);
    if (!m) return null;
    const value = parseFloat(m[1]);
    if (!isFinite(value) || value <= 0) return null;
    const unit = (m[2]?.toLowerCase() === "%" ? "%" : "px") as "px" | "%";
    return { value, unit };
  }

Also add a small helper to read an inline CSS property from a style attribute
string (reuse the existing style parser if one already exists; if parseCssStyle
exists and returns a map, use it instead of duplicating):

  function readInlineStyleProp(node: AstNode, prop: string): string | null {
    if (node.type !== "element") return null;
    const style = node.attrs?.style;
    if (!style || typeof style !== "string") return null;
    const re = new RegExp(`(?:^|;)\\s*${prop}\\s*:\\s*([^;]+)`, "i");
    const m = style.match(re);
    return m ? m[1].trim() : null;
  }

#193 — Tables: size to content, respect explicit cell widths (best-effort).

1. In styles.table, ADD:  alignSelf: "flex-start"  and  width: "auto"
   (keep existing borders/marginBottom). This stops the table from filling the
   whole page.

2. In renderTableAst, for EACH cell compute an optional explicit width from
   cell.attrs.width OR inline CSS "width" (via readInlineStyleProp + parseLen).
   Build a per-cell style:
     - If a % width is found: apply { flexGrow: 0, flexBasis: `${value}%` }.
     - If a px width is found AND it is reasonable (<= ~500, i.e. not absurdly
       larger than a page): apply { flexGrow: 0, width: value }.
       If the px value is absurd (> ~500), ignore it (fall back to default).
     - If NO explicit width: keep the current behavior for that cell
       (flexGrow: 1, flexBasis: 0 — do not add anything).
   Merge this per-cell width style into the existing cell style array AFTER
   styles.tableCell so it overrides flex only when a width was found. Do not
   change borders, padding, header styling, or the row/cell iteration logic.

#194 — Images: respect pasted dimensions, stop forced upscaling.

1. In styles.image, REMOVE  height: 120. Keep maxWidth: "100%" and
   objectFit: "contain". Do NOT set a fixed height.

2. In renderImageAst, read width/height from node.attrs.width, node.attrs.height,
   and inline CSS "width"/"height" (via readInlineStyleProp + parseLen). Build a
   per-image style object:
     - width: if % -> `${value}%`; if px and reasonable (<= ~500) -> value; else omit.
     - height: same rule; else omit (let it scale by aspect ratio).
     - Always include maxWidth: "100%" and objectFit: "contain".
     - If NEITHER width nor height is found, render with only
       { maxWidth: "100%", objectFit: "contain" } — no forced height, no upscaling.
   Apply this computed style to the <Image>. Keep the existing src guard
   (data: or http(s) only) unchanged.

Show both diffs (helpers + table changes + image changes). No auto-apply.
