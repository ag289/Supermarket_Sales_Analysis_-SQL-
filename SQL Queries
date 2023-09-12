/* Looking at the different columns */
SELECT * FROM sales;

/* Grouping gross income of each product line to obtain the sum of the gross income for each product_line */
SELECT product_line, ROUND(SUM(gross_income), 2) AS product_income
FROM sales
GROUP BY product_line
ORDER BY product_income DESC;


/* Convert date column into datetime datatype and calculate the total sales for each product line for each month */
UPDATE sales
SET date = STR_TO_DATE(date, '%m/%d/%Y');
ALTER TABLE sales
MODIFY date DATETIME;
SELECT product_line, DATE_FORMAT(date, '%Y-%m') AS month, SUM(total) AS total_sales
FROM sales
GROUP BY product_line, month;

/* Finding the average gross margin percentage for each product line */
SELECT product_line, AVG(gross_margin_percentage) AS avg_gross_margin
FROM sales
GROUP BY product_line
ORDER BY avg_gross_margin DESC;

/* calculating the total quantity of each product line for each invoice. 
Then, we're taking the average of those quantities for each product line to get the overall average quantity per product line. */
SELECT product_line, AVG(quantity) as avg_quantity
FROM sales
GROUP BY product_line
ORDER BY avg_quantity DESC;

/* Calculating the top 10 product lines with the highest average gross income */
SELECT branch, product_line, SUM(quantity) AS total_quantity, AVG(gross_income) AS avg_gross_income
FROM sales
GROUP BY branch, product_line
ORDER BY avg_gross_income DESC
LIMIT 10;

/* CTE Query to create a temporary table top_products containing the top 3 selling product lines by total sales, and jons it with the sales table to
return all sales records for those top 3 product lines */
WITH top_products AS (
    SELECT product_line, SUM(total) as total_sales
    FROM sales
    GROUP BY product_line
    ORDER BY total_sales DESC
    LIMIT 3
)
SELECT s.product_line, s.date, s.total
FROM sales s
INNER JOIN top_products tp
ON s.product_line = tp.product_line;

/* Pivot product line sales data in the sales table, group total sales of each product line by branch */
SELECT branch,
  SUM(CASE WHEN product_line = 'Electronic accessories' THEN total END) AS electronic_accessories_sales,
  SUM(CASE WHEN product_line = 'Fashion accessories' THEN total END) AS fashion_accessories_sales,
  SUM(CASE WHEN product_line = 'Food and beverages' THEN total END) AS food_and_beverages_sales,
  SUM(CASE WHEN product_line = 'Sports and travel' THEN total END) AS sports_and_travel_sales,
  SUM(CASE WHEN product_line = 'Health and beauty' THEN total END) AS health_and_beauty_sales,
  SUM(CASE WHEN product_line = 'Home and lifestyle' THEN total END) AS home_and_lifestyle_sales
FROM sales
GROUP BY branch
