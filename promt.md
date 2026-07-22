Frontend only. Single file: frontend/src/app/review-queue/page.tsx
Do NOT modify any other file. Do NOT change any existing state, effect, filtering, or backend call. Do not plan. Just apply.

UAT #141: Add a visible "Sample Name" dropdown filter to the Review Queue page. All the wiring already exists — state (sampleSelected, sampleOptions), the API call that passes sampleId, and the search box that AND-filters within the selected sample. The ONLY thing missing is the dropdown control itself.

Add a "Sample Name" dropdown in the existing controls area, next to the "My View" dropdown (same visual grouping/styling as the other controls). It must:
- bind its value to the existing sampleSelected state (value={sampleSelected ?? ""})
- populate options from the existing sampleOptions state
- have a default first option "All Samples" with value ""
- onChange call setSampleSelected(e.target.value || null)

Use this control (match the styling/label pattern of the existing My View select — adjust className to be consistent with the other controls on the page):

  <div className="w-full md:w-80">
    <label className="block text-xs text-slate-600 mb-1">Sample Name</label>
    <select
      className="bg-white w-full border-2 border-[#1F3864] rounded-md px-2 py-2 text-sm"
      value={sampleSelected ?? ""}
      onChange={(e) => setSampleSelected(e.target.value || null)}
    >
      <option value="">All Samples</option>
      {sampleOptions.map((o) => (
        <option key={o.value} value={o.value}>{o.label}</option>
      ))}
    </select>
  </div>

Do NOT change the search box, My View, page size, the load effect, or getReviewQueuePage. The existing effect already re-fetches when sampleSelected changes, and the search box already ANDs within the selected sample.

Run read-only TypeScript diagnostics on this file only.
