Fix a UI alignment issue in the Help Tips "Add New Help Tip" panel.

Read this file first:
frontend/src/app/maintenance/help-tips/page.tsx

CURRENT ISSUE:
The Save and Cancel buttons are currently positioned at the top-right 
of the "Add New Help Tip" panel, in the same row as the Form/Topic 
field labels. This looks misaligned and confusing since the buttons 
appear before the user has even filled in the content below.

FIX NEEDED:
Move the Save and Cancel buttons to the BOTTOM of the "Add New Help Tip" 
panel, positioned AFTER the rich text editor (Help Tip Content section), 
right-aligned, similar to a standard form footer.

Layout should be:
1. Form field + Topic field (side by side, top)
2. Help Tip Content (HTML) label + rich text editor toolbar + editor box
3. Helper text: "Form and Topic are required..."
4. Save and Cancel buttons (right-aligned, below the helper text)

Keep the same button styling (green Save button, outline Cancel button).
Do not change any other functionality, only reposition these two buttons.

Show me the exact lines changed.
