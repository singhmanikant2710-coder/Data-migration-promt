Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

After a successful CRM finding delete, trigger the same data refresh that Save uses 
(cache-busting router.replace), so the UI updates like it does after Save.

Currently the delete handler calls deleteCrmFinding then deleteRow, but does no refresh.

Changes:
1. Import useRouter and usePathname from "next/navigation" (useSearchParams is 
   already imported). Get: const router = useRouter(); const pathname = usePathname(); 
   const sp = useSearchParams();  (reuse existing sp if already present).

2. In the trash onClick handler, AFTER "deleteRow(row.id);" on successful delete, add 
   the same refresh Save uses:

     try {
       for (let i = sessionStorage.length - 1; i >= 0; i--) {
         const k = sessionStorage.key(i);
         if (k && k.startsWith("reviewQueue:")) sessionStorage.removeItem(k);
       }
     } catch {}
     const nextParams = new URLSearchParams(sp?.toString() ?? "");
     nextParams.set("t", String(Date.now()));
     router.replace(`${pathname}?${nextParams.toString()}`, { scroll: false });

Modify ONLY CrmFindingsAndRatingsSection.tsx. Do not touch page.tsx or any other file.
