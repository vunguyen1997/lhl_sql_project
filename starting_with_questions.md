**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

```sql

SELECT city, country, transactionrevenue
FROM all_sessions
GROUP BY city, country, transactionrevenue
ORDER BY transactionrevenue DESC 
LIMIT 10;

```


Answer: The highest transaction revenue comes from the United States and the city is Other ( due to API limitations ).  
These results show that U.S. cities dominate the site’s revenue generation.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

```sql

SELECT city, country, 
ROUND(AVG(CAST(productquantity AS numeric)), 2) AS avg_products_ordered
FROM all_sessions
GROUP BY city, country
ORDER BY avg_products_ordered DESC;

```

Answer: The average number of products ordered per visitor in each city and country is extremely low in most cases.   
That means most visitors do not order products.


**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

```sql

SELECT city, country, v2productcategory AS product_category, 
COUNT(*) AS order_count
FROM all_sessions
GROUP BY city, country, product_category
ORDER BY order_count DESC;

```
Answer: There is a noticeable pattern in the types of products ordered across different cities and countries. Most visitors tend to purchase items from categories like “Apparel” (especially men’s and kid’s clothing) and “Shop by Brand”, with a strong focus on Google and YouTube branded merchandise.

This indicates a general interest in branded lifestyle products, and suggests that promotional items tied to well-known brands are more popular among site visitors regardless of location


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

```sql

WITH product_revenue AS (
  SELECT city, country, v2ProductName AS product_name, 
  COUNT(*) as order_count
  FROM all_sessions
  GROUP BY city, country, product_name
),
ranked_products AS (
  SELECT *,
  ROW_NUMBER() OVER (PARTITION BY city, country ORDER BY order_count DESC) AS rn
  FROM product_revenue
)
SELECT city, country, product_name, order_count
FROM ranked_products
WHERE rn = 1
ORDER BY order_count DESC;

```

Answer: Across different countries and cities, the top-selling products are mostly branded merchandise from YouTube and Google.

This suggests that visitors are most interested in recognizable, brand-associated items, and these products consistently lead in sales regardless of location. The popularity of YouTube and Google-branded gear appears to be a unifying trend across regions.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```sql

SELECT city, country,
SUM(CAST(totaltransactionrevenue AS numeric)) AS total_revenue
FROM all_sessions
GROUP BY city, country
ORDER BY total_revenue DESC;

```

Answer: The majority of revenue comes from the United States, especially cities like San Francisco, Sunnyvale, Atlanta, etc... 
A significant portion also comes from "Other", likely due to tracking limits.

Overall, revenue is highly concentrated in a few U.S. regions, while most other countries contribute minimally. This suggests strong brand engagement in tech-focused areas.







