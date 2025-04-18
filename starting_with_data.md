**Question 1: Which marketing channels generate the highest total revenue per country?**

SQL Queries:

```sql

-- Aggregate total revenue by country and channel
WITH revenue_by_channel AS (
  SELECT
    country,
    channelGrouping,
    SUM(COALESCE(totalTransactionRevenue, 0)) AS total_revenue
  FROM all_sessions
  GROUP BY country, channelGrouping
),

-- Rank channels by revenue within each country
ranked_channels AS (
  SELECT *,
         ROW_NUMBER() OVER (PARTITION BY country ORDER BY total_revenue DESC) AS rank
  FROM revenue_by_channel
)

-- Get top marketing channel per country
SELECT country, channelGrouping, total_revenue
FROM ranked_channels
WHERE rank = 1
ORDER BY total_revenue DESC;

```

Answer: Most countries generated little to no revenue from any marketing channel. However, in countries with notable sales, the Direct channel contributed the most revenue.

This suggests that users who visit the site directly are more likely to make purchases. Meanwhile, Organic Search has contributed to revenue in Canada, while Referral channels have had an impact on revenue in the United States.



**Question 2: Which visitors spent the most time on the site, and what was their average session duration?**

SQL Queries:

```sql

-- Get total and average time on site per visitor
SELECT
  fullVisitorId,
  SUM(COALESCE(timeOnSite, 0)) AS total_time_on_site,
  AVG(COALESCE(timeOnSite, 0)) AS average_time_on_site
FROM all_sessions
GROUP BY fullVisitorId
ORDER BY total_time_on_site DESC;

```


Answer: Some visitors spent a significantly higher amount of time on the site, with the top user spending over 5,500 seconds in total. These users may be more interested in exploring products or content and could be valuable for targeted marketing.



**Question 3: Which cities have the highest average number of pageviews per session?**

SQL Queries:

```sql

-- Calculate the average number of pageviews per session in each city
SELECT
  country,
  city,
  COUNT(*) AS total_sessions,
  SUM(pageviews) AS total_pageviews,
  AVG(pageviews) AS avg_pageviews_per_session
FROM all_sessions
GROUP BY country, city
ORDER BY avg_pageviews_per_session DESC;

```

Answer: Some cities show a noticeably higher average number of pageviews per session, suggesting that users in those locations are more engaged and explore more content.

This could be due to more effective local marketing, user interest, or cultural differences in browsing behavior.
