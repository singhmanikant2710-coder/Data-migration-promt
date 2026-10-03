Follow-up (same task, do not commit):
Images are capped by width only. A tall image (e.g. 800x3000 px → 540x2025pt) exceeds the page content height and will clip or break pagination.

Fix: in computeImageLayout, also cap height to the available page content height for that report (portrait vs landscape, from the same per-report constants you already pass — add an optional availableHeight alongside availableWidth, or derive it from the same layout constants). Scale both dimensions proportionally so aspect ratio is kept. When height is unknown/unavailable, use a conservative default that fits the smallest page used (landscape).

Also confirm: unitless width attributes (Excel <col width="64">, <td width="120">) are treated as px (× 0.75), not pt.

Report:
- ADDED / CHANGED lines (file:line)
- Results for: 800x3000 px image (portrait + landscape), 3000x1500, 80x40, image with only height declared, image inside a narrow table cell
- Build result (tsc + npm run build)
- git status
Do not commit or push.
