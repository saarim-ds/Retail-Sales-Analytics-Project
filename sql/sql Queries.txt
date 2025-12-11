-- 1. Average discount per category
SELECT category,
       AVG(discount) AS average_discount,
       SUM(sale_price) AS total_sales
FROM retail
GROUP BY category
ORDER BY average_discount DESC;


-- 2. Month-wise sales trend
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month,
       SUM(sale_price) AS sales
FROM retail
GROUP BY month
ORDER BY sales DESC;


-- 3. Profit by category
SELECT category,
       SUM(profit) AS total_profit
FROM retail
GROUP BY category
ORDER BY total_profit DESC;


-- 4. Profit margin by sub-category
SELECT sub_category,
       SUM(sale_price) AS sales,
       SUM(profit) AS profit,
       ROUND(SUM(profit)/SUM(sale_price)*100,2) AS margin
FROM retail
GROUP BY sub_category
ORDER BY margin DESC;


-- 5. Region-wise profit
SELECT region,
       SUM(profit) AS total_profit
FROM retail
GROUP BY region
ORDER BY total_profit DESC;


-- 6. City with highest sales
SELECT city,
       SUM(sale_price) AS sales
FROM retail
GROUP BY city
ORDER BY sales DESC;


-- 7. Top 5 highest selling products in each region
WITH cte AS (
   SELECT region,
          product_id,
          SUM(sale_price) AS sales,
          ROW_NUMBER() OVER (PARTITION BY region ORDER BY SUM(sale_price) DESC) AS rn
   FROM retail
   GROUP BY region, product_id
)
SELECT region, product_id, sales
FROM cte
WHERE rn <= 5;


-- 8. Top 10 highest selling products overall
SELECT product_id,
       SUM(sale_price) AS sales
FROM retail
GROUP BY product_id
ORDER BY sales DESC
LIMIT 10;


-- 9. Yearly sales comparison
SELECT YEAR(order_date) AS years,
       SUM(sale_price) AS sales
FROM retail
GROUP BY years
ORDER BY years;
