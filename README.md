# Coffee-Shop-Sales-Analysis-
SLECT * FROM casestudy.coffee_shop;
LIMIT 10;
-- Checking number of records
SELECT COUNT(*)
FROM casestudy.coffee_shop;
SELECT COUNT(DISTINCT transaction_id)
FROM casestudy.coffee_shop;
-- Checking for duplicates
SELECT *,
    COUNT(*)
FROM casestudy.coffee_shop;
GROUP BY ALL
HAVING COUNT(*) > 1;  --Data has no duplicates
-- Checking for null values
SELECT * casestudy.coffee_shop
WHERE transaction_id IS NULL OR transaction_date IS NULL OR transaction_time IS NULL OR transaction_qty IS NULL OR store_id IS NULL OR store_location IS NULL OR product_id IS NULL OR unit_price IS NULL OR product_category IS NULL OR product_type IS NULL OR product_detail IS NULL;  -- No null values


-- converting date into the correct format and adding month name, 

SELECT 
    transaction_date,
    TRIM(TO_CHAR(TO_DATE(transaction_date, 'YYYY/MM/DD'), 'Mon')) AS month_name,
    unit_price,
    REPLACE(unit_price, ',','.'),
    ROUND(REPLACE(unit_price, ',','.'),2)
   -- TO_NUMBER(ROUND(REPLACE(unit_price, ',','.')),2),
FROM casestudy.coffee_shop;

SELECT ROUND(123.4567, 2);

-- Creating time buckets and the final table

SELECT
    transaction_date,
    TO_CHAR(transaction_date, 'YYYYMM') AS month_id,
    TRIM(TO_CHAR(transaction_date, 'Mon')) AS month_name,
    COUNT(transaction_id) AS number_of_sales,
    COUNT(product_id) AS unique_products_sold,
    SUM(transaction_qty * ROUND(REPLACE(unit_price, ',','.'),2)) AS total_amount,
    product_category,
    product_detail,
    product_type,
    store_location,
    CASE
        WHEN transaction_time BETWEEN '06:00:00' AND '11:59:59' THEN 'Morning'
        WHEN transaction_time BETWEEN '12:00:00' AND '16:59:59' THEN 'Afternoon'
        WHEN transaction_time BETWEEN '17:00:00' AND '19:59:59' THEN 'Evening'
        ELSE 'Night'
    END AS time_buckets  
FROM casestudy.coffee_shop
GROUP BY ALL;
