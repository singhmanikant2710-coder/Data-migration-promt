Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

GOAL (Geoff comment #2): The main report header currently shows 
"<sampleName> - <date> - Examination - ... - <run timestamp>". Change it to 
show ONLY the run date in MM/DD/YYYY format — no time, no sample name, no 
segment/unit.

Current code builds hdrRight like:
    const hdrRight =
      headerMeta ||
      [
        filters?.sampleName || (filters?.sampleId != null ? `#${filters.sampleId}` : null),
        genOn || null,
        [filters?.segment, filters?.unit].filter(Boolean).join("/") || null
      ]
        .filter(Boolean)
        .join(" - ");

CHANGE: Replace the hdrRight construction so it outputs only the run date in 
MM/DD/YYYY. Add a small helper to format the run date:

    function formatRunDate(value?: string): string {
      const d = value ? new Date(value) : new Date();
      if (isNaN(d.getTime())) return "";
      const mm = String(d.getMonth() + 1).padStart(2, "0");
      const dd = String(d.getDate()).padStart(2, "0");
      const yyyy = d.getFullYear();
      return `${mm}/${dd}/${yyyy}`;
    }

Then set:
    const hdrRight = formatRunDate(genOn);

Also fix the DETAIL pages header (DetailTablePage) which currently renders 
new Date().toLocaleString() (includes time). Change:
    <Text style={styles.headerMeta}>{out(new Date().toLocaleString())}</Text>
to:
    <Text style={styles.headerMeta}>{out(formatRunDate())}</Text>

CONSTRAINTS:
- Do NOT change the title text, layout, or any other section.
- Do NOT remove the headerMeta element — just change what it displays.
- Only edit this one file. Show the changed lines.
