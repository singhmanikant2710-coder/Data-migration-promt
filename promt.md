File: frontend/src/app/review-history/page.tsx

Plain text yes/no answers only, no code:
1. Does the columns array entry for "customerName" set a fixed width or 
   maxWidth (e.g. width: "300px" or a Tailwind width class on the column 
   definition itself, separate from the render JSX)?
2. Does the DataTable component (or this page's table wrapper) use 
   table-layout: fixed, or is it the browser default (auto)?
3. Is there any "whitespace-nowrap" class applied anywhere in the row/cell 
   wrapper (outside the customerName render block) that could be overriding 
   the truncate behavior?

Answer only yes/no with one short note each. Do not paste code blocks.
