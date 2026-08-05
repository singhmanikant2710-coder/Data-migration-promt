Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

ROOT CAUSE (confirmed via React-PDF warning "Node of type VIEW can't wrap 
between pages and it's bigger than available page height"):
- MatrixCount's and MatrixCommitment's ROOT <View wrap={false}> cannot split. 
- For large samples (e.g. 354), the matrix now has up to 19 rows (16 data + 
  Totals + Changes + % Change) exceeding one page height, so it overflows -> 
  blank pages + suppressed footer.
- Additionally, <View break> wrapping a wrap={false} block worsens the blank-
  page emission.

FIX (minimal, no layout/logic/data changes):

=== PART A: Remove wrap={false} from the matrix ROOT views, keep it on rows ===
In BOTH MatrixCount and MatrixCommitment, the outer/root element is currently:
    return (
      <View wrap={false}>
        ...
      </View>
    );
Change the ROOT to allow wrapping (remove wrap={false} from the root only):
    return (
      <View>
        ...
      </View>
    );
IMPORTANT: Keep wrap={false} on the individual HEADER row and each DATA row 
(and Totals/Changes/%Change rows) — those inner wrap={false} props stay 
UNCHANGED. This lets the matrix split BETWEEN rows across pages (no blank 
page, no overflow), while never splitting a single row in half.

=== PART B: Fix the <View break> wrappers (avoid break wrapping wrap=false) ===
Currently:
    <View break>
      <MatrixCommitment ... />
    </View>
    <View break>
      <DistCharts ... />
    </View>

Replace the <View break> WRAPPERS with the `break` prop applied directly, so 
there is no extra wrapper View around the matrix/charts:
- For MatrixCommitment: pass break so it starts a new page without an outer 
  wrapper. Since MatrixCommitment's root is now a plain <View> (from Part A), 
  add the break prop to a lightweight leading element OR apply break to the 
  matrix's own root View. The cleanest minimal approach: keep a break using a 
  ZERO-HEIGHT breaking element right before the section:
    <View break />
    <MatrixCommitment ... />
    <View break />
    <DistCharts ... />
  (A self-closing <View break /> forces a page break at that point WITHOUT 
  wrapping the following content, avoiding the break+wrap=false nesting.)

So the final structure becomes:
    <MatrixCount ... />
    <View break />
    <MatrixCommitment ... />
    <View break />
    <DistCharts ... />
    <View style={styles.spacer} />
    <Subreport01_Count ... />
    ...

CONSTRAINTS:
- Remove wrap={false} ONLY from the two matrix ROOT views. KEEP wrap={false} 
  on all inner rows (header, data, Totals, Changes, % Change).
- Replace the two <View break>...</View> wrappers with self-closing 
  <View break /> break markers placed BEFORE MatrixCommitment and BEFORE 
  DistCharts. Do not wrap the content.
- Do NOT change the matrix contents, the Changes/% Change rows, column 
  labels, colors, data, or calculations.
- Do NOT change the section ORDER (still MatrixCount -> MatrixCommitment -> 
  DistCharts -> subreports).
- Do NOT touch pageSetup.ts, page size, margins, the footer, or backend.
- Only edit this one file. Show: the two matrix root changes (wrap removed), 
  the kept inner-row wrap={false}, and the new <View break /> markers.
