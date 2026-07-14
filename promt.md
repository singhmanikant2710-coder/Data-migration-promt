Frontend only, single file: frontend/src/app/review-status/page.tsx. Do not read other files unless explicitly told to below. Do not plan. Just apply.

Your previous change was wrong on both points. Revert whatever width change you made to the "Select Sample Name" dropdown and apply these two fixes instead.

ISSUE 1 — Dropdown control STYLE, not width.
The "Select Sample Name" dropdown currently has a thin bottom-border-only style with a small font. The "Sample Start Date" and "Sample End Date" inputs next to it have a full box style — grey/white filled background, a full border on all four sides, rounded corners, taller height, larger font.
Make the dropdown use the SAME visual style as those date inputs: same height, same font-size, same padding, same full border on all sides, same border-radius, same background. Copy the exact className from the Sample Start Date input onto the dropdown control. Do NOT change its width or its column span in the grid — leave the layout/width exactly as it is now.

ISSUE 2 — Page width.
The Review Status page renders with large white gutters on the left and right, while Review Queue fills the full content area. Open ONLY frontend/src/app/review-queue/page.tsx (read-only), find the outermost wrapper element and its container/max-width/padding classes, and apply the identical wrapper classes to the outermost wrapper of the Review Status page so it fills the content area the same way. Remove any max-width, mx-auto, or extra horizontal padding on the Review Status page that Review Queue does not have.

Do not change the counts, the status squares, the grid columns, the Bucket filter, or pagination.
