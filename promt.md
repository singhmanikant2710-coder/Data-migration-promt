Approved for selections. 
Now do FILE 2:

frontend/src/app/maintenance/cas-findings/page.tsx

When user adds new Category:
- Check if category already exists in 
  current distinct categories list
  (case-insensitive check)
- If EXISTS → show inline red error:
  "Category already exists. 
   Please enter a different name."
- If NOT exists → call existing API:
  POST /api/v1/findings/library
  body: {
    component: firstAvailableComponent,
    findingCode: "CAT-" + Date.now(),
    category: newCategoryName,
    description: "",
    guidance: ""
  }
- Refresh categories list (DISTINCT only)
- Show green toast:
  "Category '[name]' added successfully"

No new backend endpoints needed.
Confirm file path after completion.
Wait for approval before FILE 3.
