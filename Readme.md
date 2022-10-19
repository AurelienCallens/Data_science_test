# Data science test

## NBA Challenge

I made two notebooks and a presentation for this challenge: 

- 'Predict_MVP.ipynb': Train, save and evaluate the performance of random forest to predict the ranking of the NBA players on the train data 
- 'Predict_2021.ipynb': Step by step process to predict the ranking of the 2021 NBA player with the model trained in the first notebook
- 'Presentation.pdf': 5 slides to present the methodology, the choices made, the results and the perspectives

## SQL questions 

First question: 

SELECT 
    a.ad_id,
    e.event_type,
    count(*) as "count"
FROM Ads a
JOIN Events e
ON a.ad_id = e.ad_id
GROUP BY 
    a.ad_id, e.event_type;

Second question: 

WITH count_events AS(
SELECT 
    a.ad_id,
    e.event_type,
    e.date, 
    count(*) as "count"
    LAG(count(1), 1) OVER (ORDER BY date) as count_yesterday
FROM Ads a
JOIN Events e
ON a.ad_id = e.ad_id
WHERE a.status = 'active'
GROUP BY a.ad_id, e.event_type, e.date
ORDER BY e.date ASC, "count" DESC)

SELECT
    date,
    ad_id,
    event_type,
    round(  100.0 * (count - count_yesterday) / count_yesterday, 2
) AS daily_delta
FROM count_events
GROUP BY ad_id, event_type, date
ORDER BY date ASC;