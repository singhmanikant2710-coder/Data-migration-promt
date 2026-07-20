Frontend only. Single file: frontend/src/components/pdf/InitialMemoPDF.tsx
Do NOT modify HtmlRichText.tsx, ReviewPDF.tsx, FinalMemoPDF.tsx, or any other file. Do not plan. Just apply.

The Initial Memo API response now includes a `customerBackground` field containing raw HTML from the Customer Background comments (may contain tables and images). Add a Customer Background section to the Initial Memo PDF that renders it using the existing HtmlRichText component.

1) Add the type field. In frontend/src/services/api/reporting.ts, add to the InitialMemoResponse type (do not change any existing field):
     customerBackground?: string | null;

2) In InitialMemoPDF.tsx:
   - Add the import: import HtmlRichText from "../HtmlRichText";  (adjust the relative path if the component lives elsewhere; adjust to a named import if it was exported as named)
   - Add a new section rendered in the same visual style as the existing narrative sections (e.g. the Risk Rating Justification section — same sectionTitle style, same wrapper View):

       <View style={styles.section}>
         <Text style={styles.sectionTitle}>Customer Background</Text>
         <HtmlRichText html={data?.customerBackground} fallback="-" />
       </View>

   - Place it immediately BEFORE the Risk Rating Justification section (or, if a more logical position exists in the document order, place it there and state where you put it).

Do NOT change any existing section, the Paragraphs helper, or any other field. Do not convert other narratives to HtmlRichText in this step.

Run read-only TypeScript diagnostics on the changed files only.
