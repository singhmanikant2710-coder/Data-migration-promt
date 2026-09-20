Quick clarification before I finish the Checklist Questionnaire numbering — I dug into how the Review Form actually orders the checklist questions, and it turns out it's alphabetical by question text (there's no separate sequence/order field stored anywhere in the system). 

So numbering "in Review Form order" = numbering alphabetically. I can do that with no extra work, and it's technically accurate to your original ask. Two things to flag though:

1. The numbers will look somewhat arbitrary from a business standpoint — e.g. a question starting with "Are..." might get numbered before one starting with "Was..." even if it doesn't feel like it should come first logically.
2. If a checklist question's wording is ever edited later, the numbering could shift, since there's no permanent number stored — it's recalculated from alphabetical order each time.

If that's acceptable, I'll proceed with alphabetical numbering (no extra work needed). If you'd rather have a fixed, permanent sequence number that doesn't depend on alphabetical order or shift when text changes, that's a bigger change (adding a new database field and updating how questions get loaded) — let me know if that's worth doing, or if alphabetical is fine for now.

One more small thing: this report is meant to be used with a specific Sample ID selected, correct? If someone runs it without picking a sample, questions from different samples could get mixed together in the numbering, which wouldn't make sense. Should I just leave that as-is since you said this will almost always be run with a Sample selected, or should I require a Sample ID before the report can run?
