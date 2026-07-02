READ-ONLY. Do NOT edit. Report only.

I need to add validation to the Checklist tab: the comment field must be mandatory 
when answer = "No", and should be empty otherwise.

Report ONLY:

1. In ChecklistSection.tsx, show the full render for a single checklist question row 
   (the answer Select + the comments RichTextEditor together), so I can see where to 
   add validation/error display.

2. Is there any existing validation pattern in the review form (e.g. required-field 
   errors shown inline, or a validation check in handleSave in page.tsx that blocks 
   save and shows a message)? Show one example of how other required-field validation 
   is done in this codebase, if any exists.

3. When Save is clicked (handleSave in page.tsx), is there a place where section-level 
   validation runs before calling saveReview? Show it, so I know whether to add the 
   "No requires comment" check there or inline in the component.

4. How is a checklist question's answer and comment currently stored in state 
   (the q.answer and q.comments) so I can check "answer===No && empty comment"?

Report only with exact code and file paths. No edits.
