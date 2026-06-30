New plan: fix this globally for ALL tabs, not just CRM Findings — but with ZERO impact to business logic, APIs, data, or component behavior. This must be a layout-only (CSS/spacing) change.

CONTEXT (your own diagnosis): TopChromeBar.tsx is shared and rendered position: sticky top-0 with dynamic height. The content rendered BELOW it has no reserved top offset, so on scroll content slides under the bar on every tab.

REQUIRED APPROACH:
1. First, REVERT the localized CRM Findings fix you applied earlier (the paddingTop/ResizeObserver in CrmFindingsAndRatingsSection.tsx) so we don't get double offset. Confirm it's removed.
2. Find the single shared layout/wrapper where TopChromeBar and the tab content are rendered together (likely the review-info layout or page that includes TopChromeBar). The fix belongs THERE, once, so all tabs inherit it.
3. Apply ONLY a top offset equal to the header height to the content container that sits below TopChromeBar — layout/spacing only. Prefer the simplest reliable method:
   - If TopChromeBar height is effectively stable, a CSS padding-top / margin-top on the content wrapper (no JS).
   - Only if genuinely dynamic, measure via a ref on TopChromeBar's own root element (not a class-chain querySelector) and set the offset — native ResizeObserver, no packages.

STRICT RULES:
- Layout/CSS only. Do NOT change any business logic, hooks, API calls, data flow, validation, props, or component behavior anywhere.
- Do NOT alter what TopChromeBar renders or how tabs work — only add the spacing offset below it.
- No new npm/NuGet packages.
- Read-only diagnostics FIRST: tell me exactly which file(s) you'll touch and why, and confirm none of them contain business logic that would be affected. STOP and wait for my approval before editing.
- Show me the full diff before finalizing.

Report the file list + plan first. Do not edit yet.
