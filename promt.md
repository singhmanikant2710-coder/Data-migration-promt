READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/AppShell.tsx

I'm considering changing the <main> wrapper's `overflow-x-hidden` to `overflow-x-auto` so wide content scrolls instead of being clipped. Before doing so, show me ONLY (no edits):

1. The COMPLETE <main> element and its full className, plus the div directly inside it that wraps {children}. Show exactly:
   <main className="...">
     <div className="...">{children}</div>
   </main>

2. Does the <main> have a fixed height or use flex in a way where changing overflow-x could also affect vertical scroll (overflow-y-auto is already there)? Show the height/flex classes so I confirm changing overflow-x won't disturb overflow-y.

3. Is there any horizontal scrollbar styling (scrollbar-gutter-stable, scrollbar-default) that interacts with overflow-x? Show those classes.

4. Confirm: the header (with whitespace-nowrap logo+title) is a SIBLING of main (outside it), or inside main? If the header is outside main, then main's overflow-x won't fix the header clipping — only the page-content clipping. Clarify which parts are inside vs outside main.

Read once. Findings only. No edits. I want to confirm that overflow-x-hidden -> overflow-x-auto is safe and actually fixes the visible clipping.
