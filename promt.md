Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The header meta (top-right) currently shows a composite like 
"356 - <startDate> - <endDate>" (sample id + date range). Change it to show 
ONLY the report run date in MM/DD/YYYY format — no time, no sample number, 
no date range, no segment/unit. The meta font is already white 
(colors.bannerText) — keep it white.

Replace the headerCaption useMemo so it returns only the run date:
  const headerCaption = useMemo(() => {
    const raw = data?.meta?.generatedOn;
    const d = raw ? new Date(raw) : new Date();
    if (isNaN(d.getTime())) return "";
    const mm = String(d.getMonth() + 1).padStart(2, "0");
    const dd = String(d.getDate()).padStart(2, "0");
    const yyyy = d.getFullYear();
    return `${mm}/${dd}/${yyyy}`;
  }, [data]);

(If data.meta.generatedOn is not on the type, first check for any 
generatedOn/run field on data.meta and use it; otherwise fall back to 
new Date(). Do NOT use filters.sampleId / startDate / endDate anymore.)

Keep the meta <Text> and its white color (colors.bannerText) as-is; only 
the content (headerCaption) changes. Do NOT touch pageSetup.ts, page size, 
orientation, or margins. Only edit this one file. List the change.


---------‐----------------------×××××××××××××××

Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

In the per-reviewer detail table (8 columns: CUSTOMER NAME (REVIEW ID), 
CUSTOMER #, UNIT, COMMITMENT, OUTSTANDING, BANK PD, CAS PD, and the 8th 
"(STATUS NOT SELECTED)" status column), remove the 8th status column 
entirely.

This table is repeated per reviewer via reviewerOrder.map, and each section 
has a header row, data rows, and a reviewer TOTALS row. In ALL of these:
- Remove the col.d8 header <Text> (the {statusCol} / status label cell).
- Remove the col.d8 body <Text> (the fmtStatusDate(r.statusDate) cell).
- Remove the col.d8 cell in the reviewer TOTALS row.
- Move styles.tdLast to the new final column (CAS PD / col.d7) in the header, 
  body, and totals rows so the right border renders correctly.

After removal each row must have 7 columns (d1..d7). You MAY rebalance the 
7 column widths within the table so they fill the row cleanly (keep them 
summing to 100%), but do NOT change page width/orientation.

You may leave col.d8, statusCol, statusColRaw, fmtStatusDate defined but 
unused; just remove all their USAGE in the table.

Do NOT touch pageSetup.ts, page size, orientation, or margins. Only edit 
this one file. Confirm header = body = totals column count (7 = 7 = 7) and 
list every change.


--------------------------------*******************×××××××××××××

Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

Both footer occurrences currently show a centered FHN logo + right-aligned 
"Page X of Y". Reformat to a single centered line "<Report Name> • Page X of Y", 
removing the logo (mirroring the CRM Summary footer). Keep the border-top 
(styles.footer).

In BOTH footer occurrences (one inside CroProductionSummaryPage, one inside 
the second Page of the default export), replace the inner content with:
  <View style={styles.footer} fixed>
    <Text
      style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
    />
  </View>

Use the `title` variable already in scope in both places (resolves to 
"CRO Review Production"). Remove the <Image ... FHB_Logo.png ... /> and the 
footerInner/footerCenter wrappers from BOTH footers. Keep styles.footer 
(border-top) as-is.

Do NOT touch pageSetup.ts, page size, orientation, or margins. Do NOT remove 
any logo from the report HEADER — only the footer logo. Only edit this one 
file. List both footer changes.
