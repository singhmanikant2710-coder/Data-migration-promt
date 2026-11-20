-- Approved 2025 + Approving Manager Kimberly Doane
SELECT COUNT(*) FROM dbo.[02_CORE_02_Reviews] r WITH (NOLOCK)
WHERE (r.Cancelled IS NULL OR r.Cancelled = 0) AND r.Segment IS NOT NULL
  AND r.Review_approval_date BETWEEN '2025-01-01T00:00:00' AND '2025-12-31T23:59:59'
  AND LTRIM(RTRIM(r.Review_approver_name)) = 'Kimberly Doane';
