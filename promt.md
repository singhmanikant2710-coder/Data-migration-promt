Frontend only. Single file: frontend/src/components/ui/RichTextEditor.tsx
Do NOT modify any other file. Do NOT change existing buttons or execCmd. Only ADD. Do not plan. Just apply.

UAT #168: Add "Font Color" and "Font Highlight" controls to the shared RichTextEditor toolbar so they appear in all Review Form rich text fields.

1) Add two native color inputs to the toolbar, placed right after the B / I / U group (after the divider that follows Underline). Each is a small labelled swatch that applies the chosen color to the current selection via the existing execCmd helper:

   - Font Color → execCmd("foreColor", <chosen color>)
   - Font Highlight → execCmd("hiliteColor", <chosen color>)

   Use native <input type="color"> for each. Because clicking a color input can steal focus/selection from the editor, capture the editor selection before the picker changes it: call onMouseDown={(e) => { e.preventDefault(); saveCurrentSelection(); }} is not possible on a color input directly, so instead:
   - On the input's onChange, first call focusEditor() to restore the saved selection, then call execCmd with the value. The execCmd helper already calls focusEditor() internally and restores lastRangeRef, so wire it as:
       onChange={(e) => execCmd("foreColor", e.target.value)}
   and
       onChange={(e) => execCmd("hiliteColor", e.target.value)}

   Give each a small label ("A" with a color underline for font color, and a highlighter marker for highlight) or simple text labels "Color" and "Highlight" — match the existing ToolbarButton sizing/styling as closely as a color input allows. Wrap each control so it visually aligns with the other toolbar buttons.

2) Make them hideable via the existing hideButtons prop, consistent with UAT #109. Extend the hideButtons type in this file to include:
       foreColor?: boolean;
       hiliteColor?: boolean;
   and wrap the two new controls:
       {!hideButtons?.foreColor && ( ...font color control... )}
       {!hideButtons?.hiliteColor && ( ...highlight control... )}
   Default (undefined) = shown, so they appear everywhere automatically. Do NOT pass these from any consumer — leave CRM Findings and others unchanged; they simply won't hide these unless someone opts in later.

Do NOT change any existing button, the execCmd helper, saveCurrentSelection, focusEditor, or any other logic. 

Run read-only TypeScript diagnostics on this file only.
