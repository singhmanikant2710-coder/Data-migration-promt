Fix Actions column alignment in:
frontend/src/app/maintenance/selections/
page.tsx

PROBLEM:
ACTIONS header and Edit/Delete buttons 
are misaligned — buttons shifted far 
to the right.

FIX:

1. ACTIONS header <th>:
className must include:
"text-center w-40"
(remove any text-right or text-left)

2. Actions data <td>:
className must include:
"text-center w-40"
(must match th exactly)

3. Buttons inside <td>:
Wrap Edit and Delete buttons in:
<div className="flex items-center 
justify-center gap-2">
  <button>Edit</button>
  <button>Delete</button>
</div>

RULES:
- NO text-right anywhere in Actions column
- NO justify-end anywhere in Actions column
- th and td must have IDENTICAL 
  width and alignment classes
- w-40 on both th and td
- text-center on both th and td
- flex justify-center on buttons wrapper

Only modify Actions column th and td.
Do not change any other columns.
Do not change any logic or API calls.
Confirm after fix.
