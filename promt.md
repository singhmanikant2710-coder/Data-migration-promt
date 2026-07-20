Frontend only. Single file: frontend/src/components/pdf/ReviewPDF.tsx
Do NOT modify HtmlRichText.tsx or any other file. Do not plan. Just apply.

Wire the new HtmlRichText component into the Customer Background section ONLY, so tables and images from the comments field render in the CAS Linesheet PDF instead of being flattened.

1) Add the import at the top, alongside the existing imports:
     import HtmlRichText from "./HtmlRichText";
   (adjust to a named import if the component was exported as named)

2) Find this line:
     const borrowerInformation = stripHtml(ci?.backgroundNarrative || "");
   Replace it with:
     const borrowerInformationHtml = ci?.backgroundNarrative || "";

3) In the "Customer Background" section, find:
     <Text style={styles.longText}>{borrowerInformation || "-"}</Text>
   Replace it with:
     <HtmlRichText html={borrowerInformationHtml} fallback="-" />

DO NOT change any other field. Leave stripHtml itself in place and leave every other narrative field (psorInfo, ssorInfo, transactionInformation, keyRisks, riskRatingJustification, collateralInformation, covenantInformation, policyExceptionInformation, scorecardInformation) exactly as they are — they must keep using stripHtml and <Text> as today.

Run read-only TypeScript diagnostics on this file only.
