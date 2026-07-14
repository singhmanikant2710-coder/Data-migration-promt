Frontend only. File: frontend/src/app/review-status/page.tsx. Do not plan. Just apply.

There is a <style jsx> block around lines 460-477 containing :global(.rs-input ...) rules that force border: 0 !important, box-shadow: none !important, background: transparent !important on the select control. This is fighting the new `boxed` styling on SearchableSelect and causing a box-inside-a-box.

Do two things:

1) DELETE the entire <style jsx>{` ... `}</style> block (lines ~460-477).

2) At around line 420, the Select Sample Name control is wrapped in two nested divs:
     <div className="max-w-md">
       <div className="w-full rs-input">
         <SearchableSelect className="w-full" ... boxed boldOptions />
       </div>
     </div>
   Replace both wrappers with a single div:
     <div className="w-full">
       <SearchableSelect className="w-full" ... boxed boldOptions />
     </div>
   Remove "max-w-md" and remove the "rs-input" class entirely.

Keep all SearchableSelect props (options, value, onChange, placeholder, title, boxed, boldOptions) exactly as they are.

Result must be exactly ONE bordered box, matching the Sample Start Date input in height, border, radius, background and text-sm font size.
Do not touch anything else.
