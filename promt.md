Frontend only. Do NOT modify SelectField, ui.tsx, or any shared component. Do not plan. Just apply.

The backend review GET response now returns relationshipManagerNumber and portfolioManagerNumber alongside relationshipManager and portfolioManager. Wire them through so the RM/PM dropdowns show the saved value in the same "Number - Name" format as the options, instead of appending a duplicate bare-name entry.

1) frontend/src/services/api/reviews.ts
   In the CustomerInfoSection type, ADD two optional fields (do not change any existing field):
     relationshipManagerNumber?: number | null;
     portfolioManagerNumber?: number | null;

2) The hook that maps the API payload to the section (frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCustomerInfo.ts — locate it)
   Map the two new fields through to the section object, following the exact style used for relationshipManager and portfolioManager. Do not change any existing mapping.

3) frontend/src/app/review/[ecif]/review-info/components/sections/CustomerInfoSection.tsx
   Add a small local helper above the JSX:

     const toNumberName = (name?: string | null, num?: number | null) => {
       const n = (name ?? "").trim();
       if (!n) return "";
       return num != null ? `${num} - ${n}` : n;
     };

   Then compute:
     const rmDisplay = toNumberName(r?.relationshipManager, r?.relationshipManagerNumber);
     const pmDisplay = toNumberName(r?.portfolioManager, r?.portfolioManagerNumber);

   Use these for ONLY the two SelectFields:
     Relationship Manager: value={rmDisplay}  options={ensureIncludesSelected(rmOptions, rmDisplay)}
     Portfolio Manager:    value={pmDisplay}  options={ensureIncludesSelected(pmOptions, pmDisplay)}

   Keep every other prop unchanged. Do not touch any other dropdown on the page.

Note: legacy records saved before this change have a name but no number — toNumberName falls back to the bare name for those, which is correct.

Run read-only TypeScript diagnostics on the changed files only.
