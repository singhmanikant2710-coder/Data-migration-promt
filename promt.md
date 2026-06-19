Add navigation to the "Review History" tile on the home screen.

Read this file first:
frontend/src/app/CASRRHome.tsx

Find the "Review History" tile (text shows "Review History" / 
"All prior reviews").

Look at how the working tiles next to it are implemented — 
specifically "Review Queue" and "Review Status" tiles, which 
already navigate correctly when clicked.

UPDATE only the Review History tile: make it navigate to 
/review-history using the exact same pattern (Link component or 
router.push, whichever the working tiles use) as the other tiles.

Do not modify any other tile or any other part of this file.
Show me exactly which lines changed (before/after).
