Modify these files to enforce the client's Checklist rule: comment is mandatory 
when answer = "No", and comment must be empty when answer = "Yes" or "N/A".
Two files. Manual approve.

FILE 1: frontend/src/app/review/[ecif]/review-info/components/sections/ChecklistSection.tsx
- Add a helper to detect empty comment (strip HTML, trim):
    const isCommentEmpty = (html: string) => (html ?? "").replace(/<[^>]*>/g, "").trim().length === 0;
- For each question row, show an inline red error:
    - If q.answer === "No" AND isCommentEmpty(q.comments):
        "Comment is required when the answer is \"No\"."
    - If (q.answer === "Yes" || q.answer === "N/A") AND NOT isCommentEmpty(q.comments):
        "Comment must be empty unless the answer is \"No\"."
  Render it as: <div className="text-sm text-red-600 mt-1">{message}</div> under the comments editor.

FILE 2: frontend/src/app/review/[ecif]/review-info/page.tsx
- In handleSave, BEFORE calling saveReview (same place covenants validation blocks 
  with toast.showError), add Checklist validation over the checklist questions:
    - If ANY question has answer === "No" and empty comment (strip HTML + trim):
        toast.showError("Please add a comment for every checklist item answered \"No\".", { title: "Comment required" });
        block save (return + reset isSaving like other early returns).
    - Else if ANY question has answer "Yes" or "N/A" with a non-empty comment:
        toast.showError("Comments are only allowed when the answer is \"No\". Please clear comments on Yes/N/A items.", { title: "Invalid comment" });
        block save (return + reset isSaving).
- Match the existing covenants validation/toast pattern. Show how checklist data is 
  accessed in handleSave.

Modify ONLY these two files. If checklist data isn't accessible in handleSave and 
needs another file, STOP and tell me first.
