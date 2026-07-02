READ-ONLY. Do NOT edit. Report only.

For the Checklist tab, report the CURRENT implementation state (no changes). 
Answer each precisely with file paths and code:

SAVE:
1. Is UpsertChecklistAsync (in SqlReviewRepository.cs) actually wired into 
   ReviewService.SaveAsync? Show the Checklist block in SaveAsync — does it call 
   _repo.UpsertChecklistAsync with dto.Checklist? So is Checklist save fully 
   implemented end-to-end (frontend payload → service → repo → DB)?

CLIENT BUSINESS LOGIC (report if each is implemented or NOT):
2. Answer field limited to Yes / No / N/A only — is there a dropdown/select 
   restricting the answer to these 3 values in the Checklist component? Show it.
3. Comment field mandatory when answer = "No" (and empty otherwise) — is there any 
   validation enforcing this? Show it or confirm absent.
4. Only answer + comment fields editable; all other checklist fields locked at all 
   times — is this enforced in the Checklist component? Show how fields are 
   editable/locked.
5. When review is locked, Checklist (and other tabs) locked unless unlocked by 
   Manager/Director via an Unlock button — does any lock/unlock or Unlock-button 
   feature exist? Show it or confirm absent.

HELP TIPS:
6. Is the Help Tips feature integrated into the Checklist tab (or any review tab) 
   yet — i.e., are the (i)/help icons connected to the maintenance help-tips library 
   (Form + Topic + HelpTip)? Show current state or confirm not integrated.

Report only with exact code and file paths. State clearly for each item: 
IMPLEMENTED or NOT IMPLEMENTED. No edits.
