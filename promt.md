SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx. Show unified diff BEFORE applying. Do not run build. Do NOT change anything except the two things below.

GOAL: Legacy/migrated notes contain HTML entities (&quot;, &amp;, &lt;, &gt;) instead of raw characters. Decode them ONLY for DISPLAY, without touching the DB or the edit textareas or the save logic. New saves already store raw characters correctly.

CHANGE 1 — Add a decode helper near the top of the file (after imports, with other local helpers/functions, OUTSIDE the component):

function decodeHtmlEntities(s: string | null | undefined): string {
  if (!s) return "";
  return s
    .replace(/&quot;/g, '"')
    .replace(/&#39;/g, "'")
    .replace(/&apos;/g, "'")
    .replace(/&lt;/g, "<")
    .replace(/&gt;/g, ">")
    .replace(/&amp;/g, "&");
}
(&amp; is replaced LAST intentionally.)

CHANGE 2 — In the VIEW rendering only (non-edit mode), wrap the displayed notes value with the helper. Find:
    <div className="text-xs whitespace-pre-wrap text-slate-700">{internalNotes}</div>
Change to:
    <div className="text-xs whitespace-pre-wrap text-slate-700">{decodeHtmlEntities(internalNotes)}</div>

Find:
    <div className="text-xs whitespace-pre-wrap text-slate-700">{externalNotes}</div>
Change to:
    <div className="text-xs whitespace-pre-wrap text-slate-700">{decodeHtmlEntities(externalNotes)}</div>

STRICT — DO NOT TOUCH ANY OF THESE (to avoid breaking existing behavior):
- The edit-mode <textarea> elements, their value={internalNotesEdit}/{externalNotesEdit}, onChange, ref, onFocus, onKeyDown — leave completely unchanged.
- Any save/submit logic, setInternalNotes/setInternalNotesEdit, API calls, state — unchanged.
- No other JSX, no other components, no imports removed.
- Only: add the helper function, and wrap the two VIEW <div> values with decodeHtmlEntities(...).

VERIFY BEFORE SHOWING DIFF:
a) Only the helper function was added + exactly the two view <div> lines changed.
b) Edit-mode textareas and all save logic are untouched.
c) No other lines changed.

Show the unified diff. Apply nothing until I confirm.
