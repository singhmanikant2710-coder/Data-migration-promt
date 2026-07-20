Frontend only. Single file: frontend/src/components/pdf/FinalMemoPDF.tsx
Do NOT modify HtmlRichText.tsx, ReviewPDF.tsx, InitialMemoPDF.tsx, or any other file except the type file noted below. Do not plan. Just apply.

Mirror exactly what was just done for the Initial Memo.

1) In frontend/src/services/api/reporting.ts, add to the FinalMemoResponse type (do not change any existing field):
     customerBackground?: string | null;

2) In FinalMemoPDF.tsx:
   - Add the import: import HtmlRichText from "./HtmlRichText";
   - If FinalMemoPDF.tsx declares its own local FinalMemoResponse type, add customerBackground?: string | null; there too, matching what was done in InitialMemoPDF.tsx.
   - Add a new section in the same visual style as the existing narrative sections, placed immediately BEFORE the Risk Rating Justification section:

       <View style={styles.section}>
         <Text style={styles.sectionTitle}>Customer Background</Text>
         <HtmlRichText html={data?.customerBackground} fallback="-" />
       </View>

Do NOT change any existing section, the narrative paragraph-splitting logic, or any other field.

Run read-only TypeScript diagnostics on the changed files only.
