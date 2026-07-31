Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

Three changes to the CRO Review Production report, mirroring edits already 
done on the CRM PD Grade Migration report. Make ONLY these edits.

=== PART 1: Header meta — show only the run date (MM/DD/YYYY) ===

The header meta currently shows a composite: 
`${sid} - ${sd} - ${ed}${suffix}` (sample id + date range + segment/unit). 
Geoff wants ONLY the run date (MM/DD/YYYY, no time, no sample name). The 
meta font is already white (colors.bannerText) — keep it white.

Replace the headerCaption useMemo so it returns only the run date. Use the 
report's generated-on/run timestamp if available, otherwise the current 
date. Format as MM/DD/YYYY:

  const headerCaption = useMemo(() => {
    const raw = data?.meta?.generatedOn;
    const d = raw ? new Date(raw) : new Date();
    if (isNaN(d.getTime())) return "";
    const mm = String(d.getMonth() + 1).padStart(2, "0");
    const dd = String(d.getDate()).padStart(2, "0");
    const yyyy = d.getFullYear();
    return `${mm}/${dd}/${yyyy}`;
  }, [data]);

(If data.meta.generatedOn does not exist on the type, fall back to using 
new Date() only — but first check whether a generatedOn/run field is 
available on data.meta; use it if present. Do NOT pull from filters 
startDate/endDate/sampleId anymore.)

Keep the meta <Text> and its white color (colors.bannerText) exactly as-is; 
only its content (headerCaption) changes.

=== PART 2: Remove the "(STATUS NOT SELECTED)" status column ===

The reviewer detail table has 8 columns; the 8th (col.d8) is the status 
column: header shows statusCol (from data.statusColumnLabel || 
"(STATUS NOT SELECTED)"), body shows fmtStatusDate(r.statusDate). Remove 
this column entirely.

In EVERY place the detail table is rendered (it is repeated per reviewer via 
reviewerOrder.map, and there is a header row, data rows, and a reviewer 
totals row — all per reviewer):
- Remove the col.d8 header <Text> (the {statusCol} / status label cell).
- Remove the col.d8 body <Text> (the fmtStatusDate(r.statusDate) cell).
- Remove the col.d8 cell in the reviewer TOTALS row.
- Move styles.tdLast to the new final column (CAS PD / col.d7) in header, 
  body, and totals rows so the right border renders correctly.

After removal each row must have 7 columns (d1..d7). Optionally rebalance 
widths so the 7 columns fill the row cleanly (the freed ~10% from d8 can be 
distributed, e.g. give it to d1 CUSTOMER NAME or spread across d4/d5); keep 
all widths summing to 100%. Do NOT change any other table.

You may leave col.d8, statusCol, statusColRaw, and fmtStatusDate defined but 
unused if removing them cleanly is risky — but remove all their USAGE in the 
table so the column no longer renders.

=== PART 3: Footer — mirror CRM Summary footer style ===

Both footer occurrences currently show a centered FHN logo + right-aligned 
"Page X of Y". Reformat to a single centered line "<Report Name> • Page X of Y", 
removing the logo (mirroring the CRM Summary footer). Keep the border-top.

In BOTH footer occurrences (one in CroProductionSummaryPage, one in the 
second Page of the default export), replace the inner content with:

  <View style={styles.footer} fixed>
    <Text
      style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
    />
  </View>

Use the `title` variable already in scope in both places (it resolves to 
"CRO Review Production"). Remove the <Image ... FHB_Logo.png ... /> and the 
footerInner/footerCenter wrappers from both footers. Keep styles.footer 
(border-top) as-is.

CONSTRAINTS:
- Only edit this one file. Do NOT touch the backend, queries, data, or other 
  reports.
- Do NOT remove the FHN logo from the report HEADER (if any) — only the 
  footer logo.
- After edits, confirm: (a) header meta shows only MM/DD/YYYY, (b) detail 
  table header/body/totals all have 7 columns (7=7=7), (c) both footers show 
  the centered "CRO Review Production • Page X of Y" with no logo.
- List every change made.
