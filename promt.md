File to modify: frontend/src/app/review-status/page.tsx
Reference (do NOT edit, only read for pattern): frontend/src/app/review-history/page.tsx

GOAL: Make the Review Status "Completed Draft Reviews for Approval" table behave 
like Review History:
- Clicking the pdf/document icon should OPEN THE PDF MODAL (ReviewPDFModal), 
  NOT navigate anywhere.
- Clicking the borrower name should NAVIGATE to the review-info form (router.push), 
  same as History.

Currently both the icon and the borrower name in review-status are <Link> elements 
pointing to review-info, which is wrong.

STEP 1 — Add PDF modal support to review-status/page.tsx (it currently has none):
- Add the same import that review-history uses for the modal:
  import ReviewPDFModal from "@/components/pdf/ReviewPDFModal";
  (verify the exact import path by checking how review-history imports it; match it exactly)
- Add state: const [pdfRow, setPdfRow] = useState<...>(null);  
  (match the type/style review-history uses)
- Ensure useState and useRouter are imported, and a router instance exists 
  (const router = useRouter();) if not already present.
- Render the modal at the same place in JSX as review-history does:
  {pdfRow && (
    <ReviewPDFModal
      reviewId={String(pdfRow.reviewId)}
      ecif={pdfRow.ecif ?? ""}
      borrower={pdfRow.borrower ?? ""}
      onClose={() => setPdfRow(null)}
    />
  )}
  IMPORTANT: review-status row fields are named differently than review-history. 
  In review-status the row uses r.ecif (not eCifNumber) and r.borrower (not 
  customerName). Use the review-status field names for the modal props. Verify 
  the actual field names on the review-status row object before finalizing.

STEP 2 — Change the pdf/document icon from a <Link> to a button that opens the modal:
Replace the current <Link ...><DocIcon /></Link> for the icon with a button:
  <button
    type="button"
    className="text-slate-400 hover:text-[color:var(--brand-primary)] cursor-pointer inline-flex px-1 py-0 bg-transparent border-0"
    onClick={(e) => { e.stopPropagation(); setPdfRow(r); }}
    title="Open Review Summary PDF"
    aria-label="Open Review Summary PDF"
  >
    <DocIcon />
  </button>
Keep using the existing DocIcon component already used on this page.

STEP 3 — Change the borrower name from a <Link> to a button that navigates 
(mirror review-history's navigation, but using review-status field names):
Replace the current borrower-name <Link> with:
  <button
    type="button"
    className="text-[color:var(--brand-primary)] hover:underline cursor-pointer bg-transparent border-0 p-0 text-left block w-full truncate text-ellipsis whitespace-nowrap overflow-hidden pr-12"
    onClick={(e) => {
      e.stopPropagation();
      const qs = new URLSearchParams({
        section: "review-info",
        borrower: r.borrower ?? "",
        reviewId: String(r.reviewId),
        sampleId: String(r.sampleId ?? ""),
      });
      router.push(`/review/id-${r.reviewId}/review-info?${qs.toString()}`);
    }}
    title={r.borrower ?? "-"}
  >
    {r.borrower ?? "-"}
  </button>
NOTE: If the review-status row does NOT have a sampleId field, check what 
review-status currently passes; if there's no sampleId available, omit that 
param rather than guessing. Verify available fields on the row object first.

CONSTRAINTS:
- Modify ONLY frontend/src/app/review-status/page.tsx.
- Do NOT edit review-history/page.tsx or ReviewPDFModal or any other file.
- If ReviewPDFModal's prop names differ from what I wrote, use the modal's 
  actual prop names (check review-history's usage as the source of truth).
- If anything requires touching another file, STOP and ask me first.
- Remove the now-unused <Link> import only if it's no longer used anywhere else 
  in this file (check first; if Link is still used elsewhere, keep the import).

After editing, paste back: the modal import line, the pdfRow state line, the 
new icon button JSX, the new borrower button JSX, and the modal render block, 
so I can verify before testing.
