Approved. Now do FILE 3:

frontend/src/app/maintenance/covenants/page.tsx

When user adds new Category:
- Check if category already exists in 
  current distinct categories list
  (case-insensitive check)
- If EXISTS → show inline red error:
  "Category already exists. 
   Please enter a different name."
- If NOT exists → call existing API:
  POST /api/v1/covenants/library
  body: {
    code: "CAT-" + Date.now(),
    covenantCategory: newCategoryName,
    covenantType: "",
    order: 0
  }
- Refresh categories list (DISTINCT only)
- Show green toast:
  "Category '[name]' added successfully"

No new backend endpoints needed.
Confirm file path after completion.
Wait for approval before FILE 4.
