We need to fix a UAT bug in the CASR application related to Rich Text Fields / Tables and PDF rendering.

BUG:
In the Review Form, users can paste rich-text content from Word/Excel or enter formatted rich text.

When this content is rendered in the PDF/report, raw HTML markup is displayed as visible text instead of being interpreted/rendered.

Examples currently visible in the generated PDF/report:
<div>
<span style="font-size: 0.875rem;">
</span>
</div>

The actual expected output is the formatted content itself, without showing any HTML tags.

Example:
Current:
"Annual review tracking ... <span style="font-size: 0.875rem;">05/06/2026</span> ..."

Expected:
"Annual review tracking ... 05/06/2026 ..."

IMPORTANT:
Do NOT simply strip all HTML tags blindly if that would destroy tables, line breaks, bold/italic formatting, alignment, lists, or other supported rich-text formatting.

We need the smallest safe fix in the existing PDF/report rendering pipeline.

REQUIREMENTS:

1. FIRST inspect the repository and identify the exact code path responsible for:
   - Review Form rich-text data
   - API response/model mapping
   - PDF/report generation
   - HTML/rich-text conversion/rendering
   - Any existing HTML sanitizer/parser/helper

2. Do NOT guess file paths.
   Search the repository and provide the exact file path(s) and exact method/function names before editing.

3. Trace one affected field end-to-end:
   UI Rich Text
      -> API
      -> backend/model
      -> PDF/report generation
      -> final rendered PDF

4. Determine why HTML such as:
   <div>...</div>
   <span ...>...</span>
   is being treated as plain text instead of HTML/rich text.

5. Prefer using the application's EXISTING HTML/rich-text parser or rendering utility if one already exists.
   Do not introduce a new dependency unless absolutely necessary.

6. The fix must preserve supported formatting:
   - normal text
   - line breaks
   - paragraphs
   - bold
   - italic
   - lists
   - tables
   - table rows/cells
   - alignment
   - reasonable inline styles where currently supported

7. For unsupported or unsafe HTML, handle it safely rather than exposing raw markup in the PDF.

8. Pay special attention to HTML entities and encoded content.
   Make sure content is not being:
   - HTML-encoded twice
   - decoded too late
   - converted to plain text before PDF rendering
   - escaped before being passed to the HTML renderer

9. Do NOT change database schemas, API contracts, save logic, or unrelated review functionality unless the root cause absolutely requires it.

10. Do NOT modify existing save/update behavior.
    This is primarily a rendering/output fix.

11. Do NOT break existing PDF functionality for:
    - plain text fields
    - rich text fields
    - tables
    - images
    - scorecard sections
    - Covenants
    - Policy Exceptions
    - Risk Rating Justification
    - CRM Findings
    - other Review Form sections

12. Before editing, inspect the existing implementation and explain:
    - exact root cause
    - exact file path
    - exact method
    - why the current implementation outputs raw HTML
    - safest minimal fix

13. Then make ONLY the minimal targeted code change.

14. After the change:
    - build the affected project
    - fix only errors introduced by this change
    - do not refactor unrelated code

15. Validate these cases:

CASE 1:
Plain text
Expected: unchanged.

CASE 2:
Rich text containing <div> and <span>
Expected: HTML tags are not visible; text and supported formatting render correctly.

CASE 3:
Rich text copied from Microsoft Word
Expected: no raw HTML tags in PDF and no unnecessary formatting corruption.

CASE 4:
HTML table copied from Excel/Word
Expected: table remains a table and does not stretch/break unexpectedly.

CASE 5:
Text containing:
<, >, &, quotes
Expected: displayed correctly and safely.

CASE 6:
Existing PDF sections that already work
Expected: no regression.

IMPORTANT SAFETY RULE:
Do NOT make broad changes to the PDF renderer.
Do NOT replace the existing PDF generation library.
Do NOT rewrite the rich-text system.
Do NOT modify unrelated components.
Do NOT change API/database contracts.
Do NOT change save logic.

Use the smallest possible fix at the point where rich-text HTML is incorrectly being converted/rendered as plain text.

Before making any change, inspect the relevant files and give me:

1. Exact file path
2. Exact method/function
3. Root cause
4. Current behavior
5. Proposed minimal change

Then edit the code.

After editing, give me a concise change summary with:
- File changed
- Method changed
- Exact fix
- Why existing functionality is preserved
- Build/test result
