TASK: Make the Borrower Name clickable on the Review History screen so it navigates to the Review Form, exactly like the Review Queue screen does.

EDIT ONLY THIS FILE: frontend/src/app/review-history/page.tsx
Do NOT add any npm package. Do NOT touch any other file. If another file seems to need changes, STOP and ask.

STEP 1 — Ensure router is available (top of the page component):
- If not already imported, add:  import { useRouter } from "next/navigation";
- Inside the page component body (where useState/setPdfRow etc. are declared), if not already present, add:  const router = useRouter();
- Do not duplicate if they already exist.

STEP 2 — Replace ONLY the plain borrower-name span in the "customerName" column render (currently around line 176).

FIND (exact):
        <span className="text-slate-800 font-medium" onClick={(e) => { e.stopPropagation(); }}>{r.customerName ?? "-"}</span>

REPLACE WITH:
        <button
          type="button"
          className="text-slate-800 font-medium hover:underline cursor-pointer bg-transparent border-0 p-0 text-left"
          onClick={(e) => {
            e.stopPropagation();
            const qs = new URLSearchParams({
              section: "review-info",
              borrower: r.customerName ?? "",
              reviewId: String(r.reviewId),
              sampleId: String(r.sampleId),
            });
            router.push(`/review/id-${r.reviewId}/review-info?${qs.toString()}`);
          }}
        >
          {r.customerName ?? "-"}
        </button>

STEP 3 — Do NOT modify the existing PDF/linesheet <Button> and its <svg> (lines ~177-193). Leave them exactly as-is.

After the change, confirm the file compiles and report only the edited region.
