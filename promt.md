-- Poora help tips library dekho — kitne, kya Form/Topic
SELECT [Help_tip_id],
       LTRIM(RTRIM([Help_tip_form])) AS form,
       LTRIM(RTRIM([Help_tip_topic])) AS topic,
       LEFT([Help_tip], 60) AS tip_preview
FROM dbo.[03_LIBRARY_06_Help Tips] WITH (NOLOCK)
ORDER BY form, topic;
