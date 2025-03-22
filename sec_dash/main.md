## init
```sql
CREATE TABLE PIZZA_SALES.pizza_sales (
    pizza_id INT PRIMARY KEY,
    order_id INT,
    pizza_name_id VARCHAR(127),
    quantity INT,
    order_date DATE,
    order_time TIME,
    unit_price DECIMAL(10,2),
    total_price DECIMAL(10,2),
    pizza_size CHAR(1),
    pizza_category VARCHAR(127),
    pizza_ingredients TEXT,
    pizza_name VARCHAR(127)
);

ALTER TABLE PIZZA_SALES.pizza_sales
    ALTER COLUMN pizza_size TYPE CHAR(3);

-- View data style
SHOW datestyle;

-- Change date format
SET datestyle = 'ISO, DMY';

-- Do the import

-- RESET DATE FORMAT
SET datestyle = 'ISO, MDY';

ALTER TABLE PIZZA_SALES.pizza_sales
    ALTER COLUMN order_date TYPE TEXT;

ALTER TABLE PIZZA_SALES.pizza_sales
    ALTER COLUMN order_time TYPE TEXT;

ALTER TABLE PIZZA_SALES.pizza_sales
    ALTER COLUMN order_date TYPE DATE 
    USING TO_DATE(order_date, 'DD-MM-YYYY');

ALTER TABLE PIZZA_SALES.pizza_sales
    ALTER COLUMN order_time TYPE TIME
    USING order_time::TIME;



```

# PROBLEM STATEMENT

## KPI'S REQUIREMENT

We need to analyze key indicators for our pizza sales data to gain insights into our business performance. Specifically, we want to calculate the following metrics:

1. **Total Revenue**: The sum of the total price of all pizza orders.
2. **Average Order Value**: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
3. **Total Pizzas Sold**: The sum of the quantities of all pizzas sold.
4. **Total Orders**: The total number of orders placed.
5. **Average Pizzas Per Order**: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

```sql

-- 1
SELECT SUM(TOTAL_PRICE)
FROM PIZZA_SALES.PIZZA_SALES; -- 817860.05

-- 2
SELECT SUM(TOTAL_PRICE) / (
SELECT COUNT(DISTINCT ORDER_ID)
FROM PIZZA_SALES.PIZZA_SALES
)
FROM PIZZA_SALES.PIZZA_SALES -- 38.3072622950819672

-- 3
SELECT SUM(QUANTITY)
FROM PIZZA_SALES.PIZZA_SALES -- 49574

-- 4
SELECT COUNT(DISTINCT ORDER_ID)
FROM PIZZA_SALES.PIZZA_SALES -- 21350

-- 5
SELECT CAST(SUM(QUANTITY) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT ORDER_ID) AS DECIMAL(10,2))
FROM PIZZA_SALES.PIZZA_SALES  -- 2.3291134751773049180327868852459


```

# PROBLEM STATEMENT

## CHARTS REQUIREMENT

We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:

1. **Daily Trend for Total Orders:**

   Create a bar chart that displays the daily trend of total orders over a specific time period. This chart will help us identify any patterns or fluctuations in order volumes on a daily basis.

2. **Monthly Trend for Total Orders:**

   Create a line chart that illustrates the hourly trend of total orders throughout the day. This chart will allow us to identify peak hours or periods of high order activity.

3. **Percentage of Sales by Pizza Category:**

   Create a pie chart that shows the distribution of sales across different pizza categories. This chart will provide insights into the popularity of various pizza categories and their contribution to overall sales.

```sql
-- 1
SELECT TO_CHAR(ORDER_DATE, 'DAY'), COUNT(DISTINCT ORDER_ID)
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY TO_CHAR(ORDER_DATE, 'DAY')
ORDER BY COUNT(ORDER_ID) DESC

-- 2
SELECT TO_CHAR(ORDER_DATE, 'MONTH'), COUNT(DISTINCT ORDER_ID)
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY TO_CHAR(ORDER_DATE, 'MONTH')
ORDER BY COUNT(ORDER_ID) DESC

-- 3
SELECT PIZZA_CATEGORY, CAST(SUM(TOTAL_PRICE) / (
	SELECT SUM(TOTAL_PRICE)
	FROM PIZZA_SALES.PIZZA_SALES
) * 100 AS DECIMAL(10,2))
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_CATEGORY

SELECT PIZZA_CATEGORY, CAST(SUM(TOTAL_PRICE) / (
	SELECT SUM(TOTAL_PRICE)
	FROM PIZZA_SALES.PIZZA_SALES
) * 100 AS DECIMAL(10,2))
FROM PIZZA_SALES.PIZZA_SALES
WHERE ORDER_DATE BETWEEN '2015-01-01' AND '2015-01-31'
GROUP BY PIZZA_CATEGORY

```

4. **Percentage of Sales by Pizza Size:**

   Generate a pie chart that represents the percentage of sales attributed to different pizza sizes. This chart will help us understand customer preferences for pizza sizes and their impact on sales.

5. **Total Pizzas Sold by Pizza Category:**

   Create a funnel chart that presents the total number of pizzas sold for each pizza category. This chart will allow us to compare the sales performance of different pizza categories.

6. **Top 5 Best Sellers by Total Pizzas Sold:**

   Create a bar chart highlighting the top 5 best-selling pizzas based on the total number of pizzas sold. This chart will help us identify the most popular pizza options.

7. **Bottom 5 Worst Sellers by Total Pizzas Sold:**

   Create a bar chart showcasing the bottom 5 worst-selling pizzas based on the total number of pizzas sold. This chart will enable us to identify underperforming or less popular pizza options.

```sql
-- 4
SELECT PIZZA_SIZE, CAST (SUM(TOTAL_PRICE) / (
	SELECT SUM(TOTAL_PRICE)
	FROM PIZZA_SALES.PIZZA_SALES
) * 100 AS DECIMAL(10,2))
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_SIZE

-- 5
SELECT PIZZA_CATEGORY, SUM(QUANTITY) TOTAL
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_CATEGORY

-- 6
SELECT PIZZA_NAME_ID, SUM(QUANTITY) TOTAL
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_NAME_ID
ORDER BY TOTAL DESC
LIMIT 5

-- 7
SELECT PIZZA_NAME_ID, SUM(QUANTITY) TOTAL
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_NAME_ID
ORDER BY TOTAL ASC
LIMIT 5

-- 6
SELECT PIZZA_NAME, SUM(TOTAL_PRICE) TOTAL
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_NAME
ORDER BY TOTAL DESC
LIMIT 5

--7 
SELECT PIZZA_NAME, SUM(TOTAL_PRICE) TOTAL
FROM PIZZA_SALES.PIZZA_SALES
GROUP BY PIZZA_NAME
ORDER BY TOTAL ASC
LIMIT 5
```