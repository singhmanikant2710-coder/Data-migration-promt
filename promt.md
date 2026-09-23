Context: .NET 8 Clean Architecture backend, CASRR project. Client's original
spec (items 4-5 of their requirements doc) asks for a monthly/at-load-time
batch process that populates Relationship_mgr_number/_name/_email and
Portfolio_mgr_number/_name/_email on dbo.[02_CORE_02_Reviews] by matching
against Distribution Parties (OfficerNumber = Employee ID for RM, PM Number =
Employee ID for PM), defaulting all three fields to NULL if no match is found
— for reviewers to assign manually via the front-end form in that case.

This is separate from and in addition to the review form's manual RM/PM
dropdown (already implemented) — this is about the existing "sample loading"
ingestion process that runs when reviews/samples are first loaded into the
system, similar to how the Data Mart Trial staging table gets ingested.

Investigate first — don't implement yet:
1. Find the existing "sample loading append queries/process" in this codebase
   — the process that currently populates review records when a new
   sample/review is loaded (likely already sets some fields from Data Mart
   Trial or similar staging tables). Tell me what it's called, where it lives,
   and what it currently does for RM/PM fields, if anything.
2. Confirm whether this is a scheduled job, an on-demand script, or triggered
   by some ingestion event — this affects how we'd add the Distribution
   Parties lookup to it.

Report back before writing any code, so we can confirm scope before
implementing.
