-- Test data: 12533 ke liye checklist questions insert (PPT se)
INSERT INTO dbo.[02_CORE_08_Checklists]
    ([Review_id], [Checklist_category], [Checklist_question], [Checklist_guidance])
VALUES
(12533, 'Field Exams', 
 'Does the frequency of the required Field Exams align with the Borrower''s risk profile and are they obtained in accordance with the credit approval and Loan/Credit Agreement?',
 'Per policy, the frequency of Field Exams are based on the assessment of the PSOR. Answer Yes, No, or N/A if Field Exam not required. If answered No include further comments.'),
(12533, 'Field Exams',
 'Is the Field Exam of appropriate quality and completed per policy requirements in terms of required content, including a collateral discussion, evaluation of reporting & controls, and summary of any noted observations or material issues?',
 'Review the field exam quality per policy requirements.'),
(12533, 'Field Exams',
 'When required, did the RM/PM provide a timely and adequate response to any material issues/recommendations/observations noted in the most recently completed Field Exam?',
 'Check RM/PM response timeliness and adequacy.');

-- Confirm insert hua
SELECT [Review_id], [Checklist_question], [Checklist_answer], [Checklist_comments]
FROM dbo.[02_CORE_08_Checklists] WHERE Review_id = 12533;
