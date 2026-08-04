READ-ONLY. Diagnostics only. Do not change anything.

Two rendering issues in the rich-text fields (affects Initial Memo, Final 
Memo, CAS Linesheet):

ISSUE 1 — "<" (less than) not rendering:
After our decodeEntities fix, "&amp;" and "&gt;" now render correctly as "&" 
and ">", but "&lt;" does NOT render as "<" — instead "<" and any text after 
it disappears entirely (nothing in its place). This suggests that after 
decoding "&lt;" to "<", the HtmlRichText parser treats "<" as the start of 
an HTML tag and swallows it plus following text.

Show me:
1. In HtmlRichText.tsx: the ORDER of operations — is decodeEntities called 
   BEFORE or AFTER the HTML is parsed/tokenized into tags? Show the main 
   flow (parse → decode, or decode → parse).
2. If decodeEntities runs BEFORE parsing, that's the bug: "&lt;" becomes "<" 
   and then the parser eats it as a tag. Confirm this ordering.
3. How the parser tokenizes tags (what it treats as a tag start "<...>") — 
   so I understand what happens to a stray "<" after decoding.

ISSUE 2 — Policy Exception paragraph breaks:
The Policy Exception Comments ([policy_exception_information]) don't show 
paragraph breaks; all other rich-text fields do. Diagnostics earlier noted 
Policy Exception uses plain <Text> + stripHtml (not HtmlRichText).

Show me:
1. The stripHtml helper in ReviewPDF.tsx (and Initial/Final if they have 
   their own) — how it handles line breaks / paragraph breaks. Does it 
   convert <br>, <br/>, <p>, </p>, or newlines into anything, or does it 
   strip them entirely (losing paragraph structure)?
2. How the Policy Exception value is rendered (plain <Text>{stripHtml(...)}</Text>?).
3. How other fields (that DO show paragraph breaks) render — they use 
   HtmlRichText which presumably handles <p>/<br> as paragraph breaks. 
   Confirm the difference.
4. Could Policy Exception simply be routed through HtmlRichText instead (like 
   the other narrative fields) to get proper paragraph breaks? Or does it 
   need special handling?

Do not edit anything. Findings only.
