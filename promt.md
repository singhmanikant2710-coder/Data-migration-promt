Bug 220 — Non-Compliant Covenants report must be rebuilt to match the Covenant Violations report design (Geoff's prototype: 09_Covenant Violations.pdf). READ-ONLY diagnosis, no edits. One pass, answer everything, STOP.

TARGET DESIGN (from Geoff's shared sample, Covenant Violations report):
- Header: dark navy banner, left = report title ("Covenant Violations"), right = sample context ("357 - 5/1/2026 - Examination - Franchise Finance") — mirroring other CRM reports.
- Body sections: "Total Borrowers with Covenant Violations" (count), "Total Exposure with Covenant Violations" ($ amount), then "Monitoring Covenant Violation Totals" table (COVENANT TYPE | COUNT | EXPOSURE, with a Totals row), "Performance Covenant Violation Totals" table (same columns, Totals row), then "Covenant Violation Details" table (CUSTOMER NAME (REVIEW ID) | STATUS | STATUS | COVENANT TYPE | THRESHOLD | EVAL DATE | RESULT | COMMITMENT), and an "APPLIED REPORT FILTERS" block on the last page.
- Footer: like all other CRM reports — shows "<Report Name> • Page X of Y" (report name + page number), NO First Horizon logo.

DIAGNOSE:
1. Find the Non-Compliant Covenants PDF component (current, wrong design). Full path. Describe its current header, footer, body structure, and the data it receives (repository/query + DTO). What's the backend query/repo feeding it?
2. Find the Covenant Violations PDF component (the reference/prototype to match). Full path. Describe its header component/structure, footer, body sections/tables, and its backend query/repo + DTO.
3. HEADER: what exact header structure does Covenant Violations use (navy banner with title left + sample context right)? Is it a shared header component other CRM reports use? Name it. What does Non-Compliant Covenants use instead?
4. FOOTER: confirm Covenant Violations' footer shows "<name> • Page X of Y" with NO logo. Confirm whether any report footer currently includes a First Horizon logo that must be excluded. What footer does Non-Compliant Covenants currently use?
5. DATA: does the Non-Compliant Covenants backend query already return everything the Covenant-Violations-style report needs (borrower count, total exposure, monitoring vs performance breakdown by covenant type with count+exposure, per-violation details with status/type/threshold/eval date/result/commitment, applied filters)? Or is the data shape different — will the backend query/DTO need changes to produce the same sections? Be specific about any missing fields.
6. Is there a SHARED CRM report layout (header + footer + section/table styles) that Covenant Violations uses and Non-Compliant Covenants should adopt? Name the shared components/styles.

REPORT: exact files + line numbers for both components, the shared header/footer components to reuse, whether the backend data already supports the target sections or needs changes, and a precise list of what must change in the Non-Compliant Covenants component (and backend, if needed) to match the prototype. Do NOT write any fix yet — just the complete change plan.
