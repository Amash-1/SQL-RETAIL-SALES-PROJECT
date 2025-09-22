Retail Sales Analysis SQL Project
Project Overview

Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

This project is part of my SQL learning journey. I worked with a retail sales dataset containing customer, product, and transaction details. The goal was to build the database from scratch, clean it, and then analyze it using SQL queries. By doing this, I was able to practice database design, data cleaning, exploratory data analysis, and business-focused SQL queries.

Objectives

Set up the database – Create and populate a retail sales database.

Data Cleaning – Check and remove incomplete records.

Exploratory Data Analysis (EDA) – Explore the dataset to understand it better.

Business Analysis – Write SQL queries to answer real business questions.

Project Structure
1. Database Setup

Created a database called p1_retail_db.

Created a table named retail_sales to store the dataset.

CREATE DATABASE p1_retail_db;

CREATE TABLE retail_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);

2. Data Exploration & Cleaning

Counted the total number of records.

Found the number of unique customers.

Listed all product categories.

Checked for null values and removed incomplete rows.

SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

DELETE FROM retail_sales
WHERE 
    sale_date IS NULL OR sale_time IS NULL OR customer_id IS NULL OR 
    gender IS NULL OR age IS NULL OR category IS NULL OR 
    quantity IS NULL OR price_per_unit IS NULL OR cogs IS NULL;

3. Data Analysis & Findings

I wrote several SQL queries to answer specific business-related questions:

All sales made on '2022-11-05':

SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';


Transactions where category = 'Clothing' and quantity > 4 in Nov-2022:

SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND quantity >= 4;


Total sales for each category:

SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1;


Average age of customers who bought Beauty products:

SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty';


Transactions with total_sale > 1000:

SELECT * FROM retail_sales
WHERE total_sale > 1000;


Number of transactions by gender in each category:

SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY category, gender
ORDER BY 1;


Best-selling month in each year (by average sale):

SELECT 
       year,
       month,
       avg_sale
FROM 
(    
    SELECT 
        EXTRACT(YEAR FROM sale_date) as year,
        EXTRACT(MONTH FROM sale_date) as month,
        AVG(total_sale) as avg_sale,
        RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
    FROM retail_sales
    GROUP BY 1, 2
) as t1
WHERE rank = 1;


Top 5 customers by total sales:

SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;


Number of unique customers per category:

SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category;


Number of orders by shift (Morning <12, Afternoon 12–17, Evening >17):

WITH hourly_sale AS
(
    SELECT *,
        CASE
            WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
            WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
            ELSE 'Evening'
        END as shift
    FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift;

Findings

The dataset shows sales spread across different customer age groups and product categories (Clothing and Beauty being notable).

There are several high-value transactions above 1000.

Some months clearly perform better, highlighting seasonal sales patterns.

Certain customers contribute heavily to overall sales.

Reports

Sales Summary: Total sales, category breakdown, and demographics.

Trend Analysis: Monthly and shift-based patterns.

Customer Insights: Top customers and unique buyers across categories.

Conclusion

This project helped me get hands-on practice with SQL by working through database setup, cleaning, EDA, and business-focused analysis. The queries I wrote gave me useful insights into sales trends, customer behavior, and product performance.