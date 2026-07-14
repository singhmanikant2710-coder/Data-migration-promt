Frontend only, single file: the Review Status page component (frontend/src/app/review-status/page.tsx). Do not read other files. Do not plan. Just apply.

1) LAYOUT: On the Review Status page only, the page content is horizontally compressed / narrower than the other pages (Review Queue, Review History). Compare the outer container/wrapper classes on this page with those on frontend/src/app/review-queue/page.tsx and make the Review Status page use the same width, max-width, and horizontal padding so it fills the content area identically. Do not change any shared layout component — fix it on this page only.

2) FIELD SIZE: The "Select Sample Name" dropdown is visually smaller (shorter height and smaller font) than the "Sample Start Date" / "Sample End Date" inputs beside it. Make the dropdown match those date inputs exactly: same height, same font size, same border radius, same vertical padding, so all three controls line up on the same baseline.

Do not change the counts, the status squares, the grid, the Bucket filter, or pagination.
