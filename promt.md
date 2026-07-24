Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/page.tsx
Do NOT modify any other file. Do NOT change TopChromeBar, the tab navigation (setSection), buildSafeInternalUrl, or ReviewDataContext. Do not plan. Just apply.

UAT #158: The Review Form header shows the wrong borrower ("TXO PARTNERS LP") for several reviews, especially after switching tabs. Root cause: the header name is read only from the URL query param, with a hardcoded fallback:

    const borrowerName = sp?.get("borrower") ?? "TXO PARTNERS LP";

When the borrower param is missing or stale, that hardcoded string is displayed instead of the actual borrower.

Fix: derive the header borrower from the loaded review payload first, falling back to the URL param only while data is still loading, and remove the hardcoded default entirely.

Replace that line with:

    const borrowerName =
      (response?.form?.customerInfo?.customerName ?? "").toString().trim() ||
      (sp?.get("borrower") ?? "").toString().trim() ||
      "";

Confirm the exact path to the customer name on the response object before applying — if it differs from response?.form?.customerInfo?.customerName, use the correct path and report it.

Keep passing borrowerName to TopChromeBar exactly as it is now. Do not change anything else in this file.

Run read-only TypeScript diagnostics on this file only.
