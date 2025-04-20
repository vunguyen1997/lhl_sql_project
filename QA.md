**What are your risk areas? Identify and describe them.**

1.	Missing or NULL Values
- Fields like productQuantity, totalTransactionRevenue, and timeOnSite contain many NULLs.
- Risk: This can skew averages or totals if not handled properly.

2.	Placeholder Text (e.g., ‘(not set)’, ‘not available in demo dataset’)
- Found in city, country, and other categorical fields.
-	Risk: Can group unrelated records together or create confusion in aggregation.

3.	Duplicate Identifiers
-	IDs are expected to be unique per record or session.
-	Risk: Repeated logging of the same session or transaction

4.	Unnormalized Revenue Values
-	Revenue values are likely stored in micros.
-	Risk: Without conversion, totals will appear larger than actual figures.

**QA Process:**

1. Checking for NULLs in key columns

   ```sql
   -- Check how many rows have missing values in key columns
   SELECT COUNT(*) AS rows_with_nulls
   FROM all_sessions
   WHERE totalTransactionRevenue IS NULL
     OR productQuantity IS NULL
     OR timeOnSite IS NULL;
   ```

2. Validate Placeholder Values

   ```sql
   -- Count placeholder values in city/country
   SELECT COUNT(*) AS placeholder_rows
   FROM all_sessions
   WHERE city IN ('(not set)', 'not available in demo dataset')
     OR country IN ('(not set)', 'not available in demo dataset');
   ```

3. Check for Duplicate Identifiers

   ```sql
    -- Check for duplicate id
    SELECT fullVisitorId, COUNT(*) AS id_duplicate
    FROM all_sessions
    GROUP BY fullVisitorId
    HAVING COUNT(*) > 1;
   ```

4. Normalize Revenue Values

   ```sql
   -- Normalize total transaction revenue by converting from micros to standard currency
   SELECT 
    city,
    country,
    SUM(CAST(totalTransactionRevenue AS FLOAT)) / 1000000 AS total_revenue_usd
   FROM all_sessions
   GROUP BY city, country
   ORDER BY total_revenue_usd DESC;
   ```
