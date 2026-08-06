READ-ONLY. Complete diagnostics. Do not change anything. This is a single 
comprehensive read to gather EVERYTHING needed to fix all client comments and 
redesign pagination in one pass.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Show me ALL of the following, verbatim where code is asked:

=== SECTION 1: MatrixCount component (full) ===
1a. The COMPLETE MatrixCount component source, verbatim — from function start 
    to end, including: the aggregation (byFromTo, colTotals, rowTotals, 
    grandTotal), per-row changes/pct, the two header rows, data rows, the 
    Totals row, and the footnote.
1b. Point out exactly where each of these renders:
    - the right-side "Totals" column header (client wants renamed "Bank PD Totals")
    - the bottom row "Totals" label (client wants renamed "CAS PD Totals")
    - the footnote text (client wants new text)
    - each column's flexBasis width (header + body)
    - each data cell's textAlign (client wants all centered)

=== SECTION 2: MatrixCommitment component (full) ===
2a. The COMPLETE MatrixCommitment component source, verbatim (same scope as 
    MatrixCount).
2b. Same render-point callouts as 1b, plus:
    - the fmt() usage and the current $0-blanking condition in colored cells 
      (client now REVERSES this: wants small values shown with 1 decimal, 
      e.g. $0.0/$0.04, NOT blanked)

=== SECTION 3: Shared helpers + styles ===
3a. fmt() function definition (verbatim) — the commitment formatter.
3b. colLabel() function.
3c. These styles verbatim: styles.page, styles.table, styles.tr, styles.trLast, 
    styles.trHeader, styles.thDark, styles.thCompact, styles.td, styles.tdCenter, 
    styles.tdLast, styles.footer, styles.spacer, styles.headerBar, 
    styles.sectionTitle.
3d. MARGINS and PAGE_SIZE / PAGE_ORIENTATION values (from pageSetup import) — 
    just the values, do not modify pageSetup.

=== SECTION 4: Page composition + pagination state ===
4a. The COMPLETE CrmPdGradeMigrationPage render (verbatim) — the full section 
    order: headerBar, spacers, MatrixCount, MatrixCommitment, DistCharts, 
    Subreport01-04, footer — with every wrap={false}, break, minPresenceAhead, 
    fixed prop shown exactly as they currently are.
4b. The DistCharts component: is it ONE <View> wrapping both PDBarChart 
    components? What is its wrap state? What is chartHeight (fixed height)? 
    Confirm whether the whole chart block is currently splittable (no wrap) — 
    which causes it to split across pages on large datasets.
4c. Where the footnote sits relative to styles.table — INSIDE the table 
    container or a sibling after it? (Client: never inside the table.)
4d. Any existing <View break /> markers — exact positions.

=== SECTION 5: Subreports (for row-shading confirmation, already done) ===
5a. Confirm Subreport01_Count and Subreport02_Commitment already have row 
    shading (green/red) — show one row's render to confirm it's intact (so 
    the redesign doesn't regress it).

=== SECTION 6: Border/overflow reference ===
6a. How do the OTHER tables (Detail table, subreports) handle cell borders so 
    they don't overflow the printable width? Show their table container + 
    last-cell border approach — the working pattern I should copy for the 
    matrices (overflow:hidden? tdLast? container border?).
6b. Printable content width = PAGE width − left margin − right margin. State 
    the exact number. Confirm matrix rows currently sum to 100% flexBasis 
    PLUS per-cell right borders (~20 cells × 1pt), exceeding the content width.

=== SECTION 7: Height budget ===
7a. Available content height per page = page height − top margin − 
    (bottom margin + footer reserve). State the exact number.
7b. Approximate heights: matrix header (2 rows), one data row, Totals row, 
    footnote, DistCharts block. So I can reason about which blocks exceed a 
    page (must be splittable) vs fit (can be atomic wrap={false}).

Do not edit anything. Provide all sections 1-7 completely. After this single 
read, I will apply ONE comprehensive fix covering: all 7 client comments 
(widths, renames, footnote, center cells, 1-decimal $ formatting, un-suppress 
small values) AND a consistent generic pagination strategy (charts atomic via 
wrap=false, matrices splittable with protected headers via minPresenceAhead, 
Totals+footnote grouped, footnote outside the table, border overflow fixed 
using the working table pattern, single page break before Commitment matrix) — 
with no dataset-specific logic. Findings only.


Thank you, Geoff! I really appreciate it. I mostly worked on CASR. I only spent a little time on CPAT to help resolve a few bugs whenever needed, but the majority of my work has been on CASR.
