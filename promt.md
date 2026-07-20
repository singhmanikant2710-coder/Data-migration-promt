Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/components/sections/CustomerInfoSection.tsx
Do NOT modify SelectField, ui.tsx, lookups.ts, or any shared component. Do NOT change any other dropdown on this page. Do not plan. Just apply.

Goal: point ONLY the "Relationship Manager" and "Portfolio Manager" dropdowns at the new Data Mart lookups. All other dropdowns (Portfolio Manager Lead, Executive Credit Officer, Senior Credit Officer, Segment, Unit, Market, etc.) must keep their current sources untouched.

New backend endpoints (already built, both return string[] formatted "Number - Name"):
  GET /api/v1/lookups/data-mart/relationship-managers
  GET /api/v1/lookups/data-mart/portfolio-managers

Changes:

1) Add two new state arrays alongside the existing ones:
     const [rmOptions, setRmOptions] = useState<string[]>([]);
     const [pmOptions, setPmOptions] = useState<string[]>([]);

2) Add ONE new useEffect (do not modify the existing getCasUserNameOptions effect — leave it exactly as is, since userOptionsAll/userOptionsCRO are still used by the other dropdowns). The new effect fetches both lists on mount, following the same style as the existing relationship segments fetch:

     useEffect(() => {
       let cancelled = false;
       (async () => {
         try {
           const [rm, pm] = await Promise.all([
             get<string[]>("/api/v1/lookups/data-mart/relationship-managers"),
             get<string[]>("/api/v1/lookups/data-mart/portfolio-managers"),
           ]);
           if (cancelled) return;
           setRmOptions(Array.isArray(rm) ? rm : []);
           setPmOptions(Array.isArray(pm) ? pm : []);
         } catch (e) {
           if (typeof console !== "undefined" && console.debug)
             console.debug("[CustomerInfo] failed to load Data Mart RM/PM options", e);
           if (!cancelled) { setRmOptions([]); setPmOptions([]); }
         }
       })();
       return () => { cancelled = true; };
     }, []);

3) Change ONLY these two SelectField option sources:
     Relationship Manager: options={ensureIncludesSelected(rmOptions, r?.relationshipManager)}
     Portfolio Manager:    options={ensureIncludesSelected(pmOptions, r?.portfolioManager)}
   Keep every other prop on those two SelectFields exactly as it is (label, value, className, section, name).

Note: ensureIncludesSelected keeps any previously saved legacy value visible even if it is not in the new list — that behaviour must be preserved.

Report the file changed and run read-only TypeScript diagnostics on that file only.
