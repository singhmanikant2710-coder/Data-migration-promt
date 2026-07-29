Frontend only. Single file: frontend/src/components/ui/RichTextEditor.tsx
Do NOT modify any other file or any existing button. Do not plan. Just apply.

UAT #168 fix: The Font Color and Highlight color inputs open the picker but do not apply color to the selected text. Two causes: (a) clicking the native color input loses the editor's text selection before onChange fires, and (b) execCommand foreColor/hiliteColor needs CSS styling mode enabled. Fix both.

1) On BOTH color <input type="color"> controls (Font Color and Highlight), add an onMouseDown that saves the current selection before focus leaves the editor. Do NOT preventDefault (that would stop the picker opening):
     onMouseDown={() => saveCurrentSelection()}

2) Change BOTH onChange handlers to restore the saved selection first, then apply:
     Font Color input:
       onChange={(e) => { const c = e.target.value; restoreSelection(); execCmd("foreColor", c); }}
     Highlight input:
       onChange={(e) => { const c = e.target.value; restoreSelection(); execCmd("hiliteColor", c); }}
   If saveCurrentSelection or restoreSelection are not in scope, use the existing selection helpers execCmd relies on (focusEditor restores lastRangeRef). The selected text must still be the active selection when execCmd runs.

3) In the execCmd helper, immediately BEFORE the existing document.execCommand(command, false, valueArg ?? "") line, enable CSS styling mode for color commands:
     if (command === "foreColor" || command === "hiliteColor") {
       try { document.execCommand("styleWithCSS", false, "true"); } catch {}
     }

Do NOT change B/I/U, the other toolbar buttons, or the rest of execCmd's logic. Run read-only TypeScript diagnostics on this file only.
