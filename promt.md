I have the full database schema. I will paste the real column list. 
Your job: find EVERY SQL column reference in the review-detail 
section queries that does NOT exist in the real schema, and report 
each one with the closest matching real column. Do NOT modify 
anything yet — just produce the full mismatch list in one pass.

Steps:
1. Read every section-query method used during a single review fetch 
   in backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs.
2. For each method, extract every column it selects and the table 
   it selects from.
3. Compare each column against the real schema below.
4. Output a table: | Method | Table | Wrong column in code | Correct 
   column in DB (closest match) |. List ALL mismatches across ALL 
   methods at once, not one at a time.

REAL SCHEMA (table : columns):

02_CORE_02_Reviews:
Sample_id, Sample_name, Review_id, Sample_date, Sample_type, 
Sample_criteria, Review_type, Locked, eCIF_number, Customer_number, 
Customer_name, Customer_size, Segment, Unit, Market, Line_of_business, 
Line_of_business_sub, Cost_center, Bank_number, Internal_portfolio, 
Portfolio_sub_segment, CCL, Special_assets, Legacy_bank, 
Relationship_mgr_number, Relationship_mgr_name, Relationship_mgr_email, 
Portfolio_mgr_number, Portfolio_mgr_name, Portfolio_mgr_email, 
Portfolio_mgr_lead_name, Portfolio_mgr_lead_email, TTBA_exposure, 
TTBA_approver_name, TTBA_approver_authority, TTBA_approval_reason, 
ECO_name, ECO_email, SCO_name, SCO_email, Last_annual_review_date, 
Next_annual_review_date, Stepped_up_servicing, Last_SUS_date, 
NAICS_code, NAICS_industry, NAICS_description, Source, Cancelled, 
Cancelled_date, Cancelled_reason, Start_date, Completed_date, 
CRO_name, CRO_email, CRO_manager_name, CRO_manager_email, 
Mentor_review_date, Mentor_name, Review_rejected_date, 
Review_rejection_notes, Review_approval_date, Review_approver_name, 
Review_initial_approval_date, Review_distributed_date, 
Review_finalized_date, Borrower_information, Transaction_information, 
Covenant_tracking_accuracy, Covenant_validation_accuracy, 
Covenant_breaches_addressed, Covenant_information, 
Policy_exception_mitigation, Policy_variance_mitigation, 
Policy_exception_information, HLT_flag, HRA_flag, HVCRE_flag, 
FDICIA_flag, SNC_flag, Regulatory_flag_tracking_accuracy, 
Regulatory_flag_information, Collateral_information, Collateral_rating, 
PSOR_information, PSOR_rating, SSOR_information, SSOR_rating, Key_risks, 
Scorecard_information, Risk_rating_justification, Bank_PD, CAS_PD, 
Risk_recognition_UNSAT, Risk_recognition_comments, Scorecard_mgmt_UNSAT, 
Scorecard_mgmt_comments, Underwriting_UNSAT, Underwriting_comments, 
Credit_servicing_UNSAT, Credit_servicing_comments, Loan_admin_UNSAT, 
Loan_admin_comments, Stretch_deal, Stretch_deal_comments, 
Review_follow_up, Review_follow_up_rationale, Reconsideration, 
Reconsideration_date, Reconsideration_description, 
Reconsideration_decision, Reconsideration_rationale, Appeal, 
Appeal_date, Appeal_description, Appeal_decision, 
Appeal_decision_rationale

Note especially: the code's "ratings query" in 
GetCrmFindingsSectionAsync / GetRiskRatingJustificationSectionAsync 
references Risk_recognition_rating, Scorecard_mgmt_rating, 
Underwriting_rating, Credit_servicing_rating, Loan_admin_rating — 
but the real table has *_UNSAT and *_comments columns, NOT *_rating. 
Identify exactly how those map.

Just give me the full mismatch table. No edits yet.
