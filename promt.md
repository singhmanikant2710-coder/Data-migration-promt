Frontend only. Two files. Do NOT modify any other file. Do NOT change any existing behaviour — the new prop must default to showing all buttons, so all OTHER editors (Customer Background, Covenants, Policy Exceptions, Scorecard, Risk Rating Justification) are completely unaffected. Do not plan. Just apply.

UAT #109: hide Img URL, Upload, Table, +Row, -Row, +Col, -Col buttons ONLY in the CRM Findings comments editor.

FILE 1: frontend/src/components/ui/RichTextEditor.tsx

1) Add an optional prop to RichTextEditorProps (below showToolbar):
     hideButtons?: {
       imgUrl?: boolean;
       upload?: boolean;
       table?: boolean;
       rowAdd?: boolean;
       rowDelete?: boolean;
       colAdd?: boolean;
       colDelete?: boolean;
     };

2) Destructure hideButtons in the component signature (add it alongside the other props).

3) Wrap exactly these seven existing toolbar buttons in conditional rendering (leave every OTHER button — B, I, U, P, H1, H2, DIV, List, Link, Clear — untouched):
     {!hideButtons?.imgUrl && (<ToolbarButton onClick={insertImageFromUrl} label="Img URL" ariaLabel="Insert image from URL" />)}
     {!hideButtons?.upload && (<ToolbarButton onClick={triggerImageFilePicker} label="Upload" ariaLabel="Upload image file" />)}
     {!hideButtons?.table && (<ToolbarButton onClick={insertTable} label="Table" ariaLabel="Insert table" />)}
     {!hideButtons?.rowAdd && (<ToolbarButton onClick={addTableRow} label="+Row" ariaLabel="Add table row" />)}
     {!hideButtons?.rowDelete && (<ToolbarButton onClick={deleteTableRow} label="-Row" ariaLabel="Delete table row" />)}
     {!hideButtons?.colAdd && (<ToolbarButton onClick={addTableColumn} label="+Col" ariaLabel="Add table column" />)}
     {!hideButtons?.colDelete && (<ToolbarButton onClick={deleteTableColumn} label="-Col" ariaLabel="Delete table column" />)}

   Do NOT remove or change any handler (insertImageFromUrl, insertTable, addTableRow, etc.) — only wrap the button rendering.

FILE 2: frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

4) On the CRM Findings comments RichTextEditor instance ONLY (the one with ariaLabel="Finding Comments"), add:
     hideButtons={{
       imgUrl: true,
       upload: true,
       table: true,
       rowAdd: true,
       rowDelete: true,
       colAdd: true,
       colDelete: true,
     }}
   Keep all its other props exactly as they are. Do NOT touch any other RichTextEditor instance in this or any other file.

Run read-only TypeScript diagnostics on both files only.
