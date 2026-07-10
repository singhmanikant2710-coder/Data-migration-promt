Hi Geoffrey,

Hope you're doing well. I'm working through the Customer Info tab UAT items and wanted to confirm a few points on UAT #31 (Review Form / Customer Info — Customer Background Comments) before implementing, so we build exactly what you expect. A couple of these touch on how saving works, so your guidance would be really helpful.

1. Field/comment auto-save (MS Access style): You asked whether edits can be saved as soon as you leave a field. Currently the form uses a common Edit → Save → Cancel model across tabs (edits are held until you click Save, and Cancel discards them). This helps guard against accidental changes. Would you like us to keep this model (and make it clearer in the UI), or do you specifically want field-level auto-save?

2. Comments table — structure editing: For the Table inserted into the comments field, would you like the ability to add/delete rows and add/delete columns after the table is created?

3. Comments table — PDF rendering: The table currently does not render in the CAS Linesheet PDF. Should we make this table render in the PDF output? (Confirming this is required, as it's a separate piece of work.)

4. Clear button: You noted the Clear button doesn't clear the comments. Could you confirm what you'd like it to do (e.g., clear all comment text, clear only formatting, etc.)? We'll align it with your expectation.

Once you confirm the above, we'll scope these and share a plan. Happy to jump on a quick call if that's easier to walk through.

Thank you for the detailed review!

Best regards,
Manikant

Hi Geoffrey,

Hope you're doing well. I'm working on UAT #29 (Review Form / Customer Info — Relationship Manager & Portfolio Manager) and wanted to confirm the approach with you before implementing, as it involves a data-source decision.

As noted in the UAT, both Relationship Manager and Portfolio Manager are currently tied to our CAS Users library, and the requirement is to connect them to FHN's Active Directory for RM/PM changes — with a fallback to a query of the 01_DATA_01_Data Mart Trial table if a direct AD connection isn't possible.

A few questions:

1. Data source: A direct Active Directory integration is a larger, separate effort (security, infrastructure, credentials). For now, would you be comfortable if we implement the Data Mart Trial fallback you outlined — sourcing the dropdowns from that table — and scope the AD integration separately later? Or is direct AD connectivity required up front?

2. Name + Number capture: Per the requirement, when a Relationship Manager is selected we'd capture/update both Relationship_mgr_name (from OfficerName) and Relationship_mgr_number (from OfficerNumber); similarly for Portfolio Manager (PMName + PM Number). Can you confirm this is the expected behavior — i.e., the dropdown shows the name, and we store both name and number behind the scenes?

3. Any preference on how the dropdown should display (name only, or name + number)?

Once you confirm, we'll proceed with the implementation. Happy to hop on a quick call if that's easier.

Thank you!

Best regards,
Manikant
