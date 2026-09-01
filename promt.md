READ-ONLY. Find how covenant Order is validated for uniqueness, and any caching in the covenants flow. Quote with paths.

CONTEXT: 
1) In legacy, covenant Order must be UNIQUE. If a user sets Order=1 when 1 already exists, legacy shows a popup: "Please only select one Order value for BlackBook display. '1' has been selected more than once. To update, first change existing value to '0'." The new app lacks this validation/popup.
2) Order = the covenant's display sequence in the BlackBook (0 = hidden, 1=first, 2=second...). Changing order on Customer View updates the BlackBook order (this works).
3) Possible caching issue: covenant order changes may not reflect immediately (like the maxMonthKey getOnce cache issue we fixed).

In the Covenants component (frontend/src/app/customer/edit/page.tsx and components/CustomerEditParts.tsx / lib/covenants.ts):
1) Find the Order dropdown/select for covenants. Quote its onChange. When a user picks an Order value, is there any check that the value is unique among the other covenants? Quote any validation, or confirm there's NONE (so duplicates are silently allowed).
2) How is order stored/tracked (covOrderMap?)? When a duplicate order is chosen, what currently happens — silently overwrite, set to 0, or nothing? Quote.
3) Is there a place to add a validation: on order change, if the new order value already exists in another covenant (and is non-zero), show a warning toast/popup and prevent/reset the change — matching legacy?
4) CACHE CHECK: Does the covenants read/order flow use any getOnce/window cache (like getMaxMonthKey did)? Search for getOnce, __bcat_cache, or cached lookups in the covenants path. Quote any caching that could make order changes not reflect immediately.

OUTPUT:
- A) The Order onChange + any uniqueness validation (or confirm none), quoted.
- B) What happens on duplicate order currently, quoted.
- C) Where to add the legacy-style uniqueness validation + warning.
- D) Any caching in covenants flow (getOnce/window cache) that needs busting, quoted.
- No fix. Findings only.
