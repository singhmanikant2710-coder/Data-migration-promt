Frontend only. File: frontend/src/app/review-status/page.tsx. Single edit. Do not plan. Just apply.

The page's content wrapper renders at only 366px wide instead of filling the available width, producing large white gutters on both sides. In the DOM it is:
  <div class="p-4 space-y-4">
and it sits inside:
  <div class="mx-auto w-full min-h-0 flex flex-col flex-1">

Find the JSX element in review-status/page.tsx whose className is exactly "p-4 space-y-4" (it is the outer wrapper of the whole page body, containing the REVIEW STATUS card).

Change its className to:
  "w-full p-4 space-y-4"

That is the only change. Do not touch any other element, any other file, or the layout component.
