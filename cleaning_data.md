What issues will you address by cleaning the data?
    Missing or Null Value
    Placeholder and Invalid Value

Solution:
    Relace placeholder values in city and country
    Fill missing numeric fields with 0 for calculation consistency
    Handle empty and other NULL text fields by labeling them "N/A"

Queries:
-- 1. Standardize placeholder values

UPDATE all_sessions
SET city = 'Other'
WHERE city IN ('(not set)', 'not available in demo dataset');

UPDATE all_sessions
SET country = 'Other'
WHERE country IN ('(not set)', 'not available in demo dataset');

-- 2. Fill NULL numeric values with 0

UPDATE all_sessions
SET totalTransactionRevenue = 0
WHERE totalTransactionRevenue IS NULL;

UPDATE all_sessions
SET transactions = 0
WHERE transactions IS NULL;

UPDATE all_sessions
SET sessionQualityDim = 0
WHERE sessionQualityDim IS NULL;

UPDATE all_sessions
SET timeOnSite = 0
WHERE timeOnSite IS NULL;

UPDATE all_sessions
SET productrefundamount = 0
WHERE productrefundamount IS NULL;

UPDATE all_sessions
SET productQuantity = 0
WHERE productQuantity IS NULL;

UPDATE all_sessions
SET productRevenue = 0
WHERE productRevenue IS NULL;

UPDATE all_sessions
SET transactionRevenue = 0
WHERE transactionRevenue IS NULL;

UPDATE all_sessions
SET itemquantity = 0
WHERE itemquantity IS NULL;

UPDATE all_sessions
SET itemrevenue = 0
WHERE itemrevenue IS NULL;

-- 3. Fill NULL text fields with 'N/A'

UPDATE all_sessions
SET currencyCode = 'N/A'
WHERE currencyCode IS NULL;

UPDATE all_sessions
SET transactionId = 'N/A'
WHERE transactionId IS NULL;

UPDATE all_sessions
SET pageTitle = 'N/A'
WHERE pageTitle IS NULL;

UPDATE all_sessions
SET eCommerceAction_option = 'N/A'
WHERE eCommerceAction_option IS NULL;

UPDATE all_sessions
SET searchkeyword = 'N/A'
WHERE searchkeyword IS NULL;
