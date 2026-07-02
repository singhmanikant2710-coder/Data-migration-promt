INSERT INTO dbo.[02_CORE_08_Checklists]
    ([Review_id], [Checklist_category], [Checklist_question], [Checklist_guidance])
VALUES
(12533, 'Field Exams', 'Field Exam frequency aligned?', 'Answer Yes/No/NA. If No, add comments.'),
(12533, 'Field Exams', 'Field Exam quality adequate?', 'Check quality per policy.'),
(12533, 'Field Exams', 'RM/PM response timely?', 'Check response timeliness.');

SELECT * FROM dbo.[02_CORE_08_Checklists] WHERE Review_id = 12533;
